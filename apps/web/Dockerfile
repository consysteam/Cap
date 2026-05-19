FROM node:20-slim

# Установлюємо pnpm глобально через npm
RUN npm install -g pnpm

WORKDIR /app

# Копіюємо конфіги монорепозиторію
COPY package.json pnpm-lock.yaml* pnpm-workspace.yaml* ./
COPY apps/web/package.json ./apps/web/

# Встановлюємо залежності для веб-частини
RUN pnpm install --filter @cap/web

# Копіюємо весь інший код
COPY . .

# Виставляємо змінні для продакшн збірки
ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

# Компілюємо проєкт
RUN pnpm --filter @cap/web build

EXPOSE 3000

# Запускаємо від імені root, щоб Railway не мав проблем із правами доступу
USER root

CMD ["pnpm", "--filter", "@cap/web", "start"]
