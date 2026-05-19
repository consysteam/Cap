FROM node:20-slim

# Установлюємо pnpm та необхідні системні утиліти
RUN npm install -g pnpm

WORKDIR /app

# Копіюємо абсолютно всі конфіги та package.json файли для збереження структури монорепозиторію
COPY package.json pnpm-lock.yaml* pnpm-workspace.yaml* turbo.json* ./
COPY apps/web/package.json ./apps/web/
COPY packages/ ./packages/

# Встановлюємо ВСІ залежності монорепозиторію (це вирішить проблему з "effect")
RUN pnpm install

# Копіюємо решту файлів коду
COPY . .

# Виставляємо змінні для продакшн збірки
ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

# Компілюємо веб-додаток
RUN pnpm --filter @cap/web build

EXPOSE 3000

USER root

CMD ["pnpm", "--filter", "@cap/web", "start"]
