# MCP-Server-TESS-XTP

Extensão XTP para o servidor MCP-TESS - Integração da API TESS com o protocolo XTP.

## Sobre

Este projeto implementa uma extensão XTP para o servidor MCP-TESS, permitindo a utilização das ferramentas da API TESS em plataformas compatíveis com XTP.

## Recursos

- **Ferramentas TESS via XTP**: Acesso às principais ferramentas da API TESS através do protocolo XTP
- **Integração com Extism**: Compilação para WebAssembly permitindo uso em diversos ambientes
- **Configuração Simples**: Configuração única da chave API para acesso a todas as ferramentas

## Instalação

```bash
# Clone o repositório
git clone https://github.com/diegofornalha/mcp-server-tess-xtp.git
cd mcp-server-tess-xtp

# Instale as dependências
npm install
```

## Compilação

```bash
# Prepare o ambiente e compile
bash prepare.sh && npm run build
```

## Ferramentas Disponíveis

Este plugin XTP expõe as seguintes ferramentas:

1. `listar_agentes_tess` - Lista todos os agentes disponíveis na API TESS
2. `obter_agente_tess` - Obtém detalhes de um agente específico
3. `executar_agente_tess` - Executa um agente com mensagens específicas
4. `listar_arquivos_agente_tess` - Lista arquivos associados a um agente

## Implantação com XTP CLI

```bash
# Faça login no XTP
xtp login

# Envie o plugin
xtp plugin push
```

## Relacionados

- [MCP-Server-TESS](https://github.com/diegofornalha/mcp-server-tess) - Implementação MCP completa para a API TESS
- [Extism](https://extism.org/) - Framework para plug-ins WebAssembly

## Licença

MIT 