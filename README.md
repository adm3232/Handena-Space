# Criar o diretório principal do projeto
mkdir handena-business
cd handena-business

# Criar as pastas principais
mkdir backend frontend mobile docs

# Estrutura do backend
mkdir -p backend/src/{controllers,models,services,middlewares,utils}
mkdir backend/tests
touch backend/.env.example backend/package.json

# Estrutura do frontend
mkdir -p frontend/src/{components,pages,assets,hooks,utils}
mkdir frontend/tests
touch frontend/.env.example frontend/package.json

# Estrutura do mobile
mkdir -p mobile/src/{components,pages,assets,hooks,utils}
mkdir mobile/tests
touch mobile/.env.example mobile/package.json

# Documentação
mkdir docs
touch docs/architecture.md docs/api-spec.md docs/setup-guide.md docs/README.md

# Arquivos gerais
touch .gitignore LICENSE CONTRIBUTING.md README.md# Handena-Space