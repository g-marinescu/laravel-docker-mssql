FROM php:8.3-fpm

# Argumente pentru user
ARG USER_ID=1000
ARG GROUP_ID=1000

# Instalează dependențele de sistem
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    gnupg2 \
    apt-transport-https \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Instalează Microsoft ODBC Driver 18 pentru SQL Server
RUN curl -fsSL https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor -o /usr/share/keyrings/microsoft-prod.gpg \
    && curl -fsSL https://packages.microsoft.com/config/debian/12/prod.list | tee /etc/apt/sources.list.d/mssql-release.list \
    && apt-get update \
    && ACCEPT_EULA=Y apt-get install -y msodbcsql18 unixodbc-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Instalează extensiile PHP pentru SQL Server
RUN pecl install sqlsrv pdo_sqlsrv \
    && docker-php-ext-enable sqlsrv pdo_sqlsrv

# Instalează alte extensii PHP necesare pentru Laravel
RUN docker-php-ext-install pdo mbstring exif pcntl bcmath gd

# Instalează Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Creează user non-root
RUN groupadd -g ${GROUP_ID} appgroup \
    && useradd -u ${USER_ID} -g appgroup -m appuser

# Setează directorul de lucru
WORKDIR /var/www/html

# Schimbă ownership
RUN chown -R appuser:appgroup /var/www/html

USER appuser