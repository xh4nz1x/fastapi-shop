# Используем официальный образ Bun для сборки
FROM oven/bun:1-alpine as build

WORKDIR /app

# Аргументы сборки для Vite
ARG VITE_API_BASE_URL
ENV VITE_API_BASE_URL=${VITE_API_BASE_URL}

# Копируем файлы зависимостей
COPY package.json bun.lockb ./

# Устанавливаем зависимости с помощью bun
RUN bun install --frozen-lockfile

# Копируем исходный код и собираем проект
COPY . .
RUN bun run build

# Финальный образ остается на nginx
FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
