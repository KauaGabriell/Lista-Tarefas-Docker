#ESTÁGIO 1: BUILD (Construção)
# Damos a este estágio o apelido de 'build-stage'.
FROM node:20-alpine AS build-stage

# Define o diretório.
WORKDIR /app

#Copia os arquivos 'package.json' e 'package-lock.json'.
COPY package*.json ./

#Instala as dependências.
RUN npm install

# Copia todo o código-fonte do frontend.
COPY . .

# Executa o script de build 'vite build', conforme seu package.json.
# Isso vai gerar uma pasta '/app/dist' (padrão do Vite).
RUN npm run build

# --- ESTÁGIO 2: SERVE (Servidor Web) ---
# Iniciamos um container novo e leve com Nginx.
FROM nginx:stable-alpine

# Copia os arquivos estáticos gerados no 'build-stage'...
# ...para dentro da pasta padrão do Nginx.
COPY --from=build-stage /app/dist /usr/share/nginx/html

# Expõe a porta 80, que é a porta padrão do Nginx.
EXPOSE 80

# O comando padrão do Nginx para iniciar o servidor.
CMD ["nginx", "-g", "daemon off;"]