# --- Estágio 1: Imagem base Node.js (Versão 20, mais segura) ---
    FROM node:20-alpine

    # --- Configuração do Ambiente ---
    WORKDIR /usr/src/app
    
    # Copiando a bagaça toda(Arquivos e Dependências)
    COPY package*.json ./
    RUN npm install
    
    #Copiando o Código-Fonte 
    COPY . .
    
    #Expondo a Porta 
    EXPOSE 3001
    
    #Comando de Inicialização 
    #Comando para iniciar a API, conforme 'package.json'.
    CMD [ "npm", "start" ]