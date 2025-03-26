# Guia de Publicação: MCP-Server-TESS-XTP no MCP.run

Este guia detalha o processo completo para publicar e atualizar o plugin MCP-Server-TESS-XTP na plataforma MCP.run, desde a configuração inicial até o processo de atualização.

## Pré-requisitos

Antes de começar, você precisa ter:

- Node.js (versão 16 ou superior)
- npm ou yarn
- CLI XTP instalada
- Uma conta no MCP.run
- Token de acesso XTP
- Git instalado (para versionamento)

## 1. Instalação da CLI XTP

A CLI XTP é necessária para compilar e publicar o plugin. Instale-a com:

```bash
curl "https://static.dylibso.com/cli/install.sh" -s | bash
```

Verifique a instalação:

```bash
xtp --version
```

## 2. Autenticação na plataforma XTP

Há duas maneiras de autenticar:

### Método 1: Login interativo

Execute o comando de login:

```bash
xtp login
```

Isso abrirá uma janela do navegador para autenticação.

### Método 2: Token de acesso (recomendado para CI/CD)

Configure o token de acesso como variável de ambiente:

```bash
export XTP_TOKEN=xtp0_SEU_TOKEN_AQUI
```

Exemplo com o token usado anteriormente:

```bash
export XTP_TOKEN=xtp0_AZXRKn90dEmxthgTLM5EwvTuX-rzoZjD7O85UpdlYsN2BFyJRCihkw
```

## 3. Configuração do projeto

### Clonando o repositório (se ainda não tiver feito)

```bash
git clone https://github.com/diegofornalha/mcp-server-tess-xtp.git
cd mcp-server-tess-xtp
```

### Instalando dependências

```bash
npm install
```

## 4. Desenvolvimento do plugin

O plugin está estruturado da seguinte forma:

- `src/main.ts`: Implementação principal das ferramentas TESS
- `src/pdk.ts`: Tipos e definições para o Plugin Development Kit
- `xtp.toml`: Configuração do plugin XTP

Ao modificar o plugin, foque no arquivo `src/main.ts` que contém as implementações das ferramentas TESS. As principais funções são:

- `callImpl()`: Chamada quando uma ferramenta é invocada
- `describeImpl()`: Fornece informações sobre as ferramentas disponíveis

### Adicionando uma nova ferramenta

Para adicionar uma nova ferramenta:

1. Adicione-a à lista `TESS_TOOLS` em `src/main.ts`
2. Implemente a lógica no switch case dentro de `callImpl()`

Exemplo:

```typescript
// Adicionar à lista TESS_TOOLS
{
  name: "nova_ferramenta_tess",
  description: "Descrição da nova ferramenta",
  inputSchema: {
    type: "object",
    properties: {
      parametro1: { type: "string", description: "Descrição do parâmetro" }
    }
  }
}

// Implementar no switch case
case "nova_ferramenta_tess":
  result = await minhaNovaFerramenta(params);
  break;
```

## 5. Compilação e teste

### Preparando o ambiente de compilação

O script `prepare.sh` instala as dependências necessárias para a compilação:

```bash
bash prepare.sh
```

### Compilando o plugin

```bash
npm run build
```

Isso criará um arquivo WebAssembly na pasta `dist/`.

## 6. Publicação no MCP.run

### Primeira publicação

Após compilar o plugin, você pode publicá-lo com:

```bash
xtp plugin push
```

Este comando utiliza as configurações do arquivo `xtp.toml` para publicar o plugin.

### Verificando a publicação

Para verificar se o plugin foi publicado corretamente:

```bash
xtp plugin view --name 'mcp-server-tess' --extension-point ext_01je4jj1tteaktf0zd0anm8854
```

## 7. Atualização do plugin

Para atualizar o plugin após modificações:

1. Faça as alterações no código
2. Commit as mudanças no Git
3. Compile novamente:
   ```bash
   bash prepare.sh && npm run build
   ```
4. Publique a nova versão:
   ```bash
   xtp plugin push
   ```

O sistema criará automaticamente uma nova versão do plugin.

## 8. Integração com MCP.run

Depois de publicado, seu plugin estará disponível no MCP.run através do URL SSE:

```
https://www.mcp.run/api/mcp/sse?nonce=XXXXX&username=XXXXX&profile=XXXXX&sig=XXXXX
```

Para usar seu perfil específico:

```
https://www.mcp.run/api/mcp/sse?nonce=T8zqU1NRGYjE_eHF3MuT_w&username=diegofornalha&profile=diegofornalha%2Ftess&sig=SBGfKwIDCeuFE52ktYCu5aNI8-LpHyOKB7IIw_BgT44
```

## 9. Resolução de problemas

### Erros comuns

1. **Erro de autenticação:**
   ```
   Error: "GET extension-points/ext_01je4jj1tteaktf0zd0anm8854" HTTP ERROR 401
   ```
   Solução: Execute `xtp login` novamente ou verifique seu token XTP.

2. **Erro de compilação:**
   ```
   Error: error generating plugin: dependency is not available: extism-py
   ```
   Solução: Execute `bash prepare.sh` para instalar as dependências.

3. **Erro de publicação:**
   ```
   Error: Failed to push plugin: ...
   ```
   Solução: Verifique se o plugin compilou corretamente e se você tem permissões.

## 10. Recursos adicionais

- [Documentação do XTP](https://xtp.dylibso.com/docs)
- [Repositório GitHub do plugin](https://github.com/diegofornalha/mcp-server-tess-xtp)
- [API TESS](https://tess.pareto.io/api)
- [MCP.run](https://www.mcp.run)

---

Este guia é mantido por Diego Fornalha. Para dúvidas ou sugestões, abra uma issue no GitHub. 