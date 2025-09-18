# المرحلة الأولى: base image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# المرحلة الثانية: build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# نسخ ملفات المشاريع
COPY CarRentalSystem.Core/ CarRentalSystem.Core/
COPY CarRentalSystem.Web/ CarRentalSystem.Web/

# استعادة الباكجات
RUN dotnet restore CarRentalSystem.Web/CarRentalSystem.Web.csproj

# بناء المشروع
WORKDIR /src/CarRentalSystem.Web
RUN dotnet build CarRentalSystem.Web.csproj -c $BUILD_CONFIGURATION -o /app/build

# المرحلة الثالثة: publish
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish CarRentalSystem.Web.csproj -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# المرحلة النهائية: run
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CarRentalSystem.Web.dll"]
