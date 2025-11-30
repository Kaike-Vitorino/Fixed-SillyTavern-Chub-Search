# SillyTavern-Chub-Search v4 Fix (2025)

## O que foi corrigido?

Esta versão corrige o problema da extensão que parou de funcionar devido às mudanças na API do Chub.ai e proteção Cloudflare implementada em 2025.

### Problema Identificado

A API do Chub.ai (`api.chub.ai`) está protegida por Cloudflare e bloqueia requisições diretas que não vêm de um navegador autenticado. Isso causa erro 403 (Forbidden) em todas as tentativas de busca e download.

### Solução Implementada (v4)

**Mudanças principais:**

1. **Priorização do endpoint de avatars**: A função `getCharacter()` agora tenta primeiro o endpoint `https://avatars.charhub.io/avatars/{fullPath}/avatar.webp`, que **não está bloqueado** e funciona de forma confiável.

2. **Fallback para API oficial**: Se o endpoint de avatars falhar (improvável), a extensão tenta a API oficial como backup.

3. **Tratamento de erros robusto**: 
   - Mensagens de erro claras quando a API está bloqueada
   - Notificações informativas sugerindo acesso direto ao chub.ai
   - Caracteres que falharem ao carregar são ignorados ao invés de quebrar toda a busca

4. **Logging melhorado**: Console logs detalhados para facilitar debugging.

## Instalação

### Método 1: Substituir arquivos manualmente

1. Navegue até a pasta de extensões do SillyTavern:
   ```
   SillyTavern/public/scripts/extensions/SillyTavern-Chub-Search/
   ```

2. Faça backup dos arquivos originais (opcional):
   ```bash
   cp index.js index.js.backup
   cp manifest.json manifest.json.backup
   ```

3. Substitua os arquivos `index.js` e `manifest.json` pelos da pasta v3

4. Reinicie o SillyTavern ou recarregue a página

### Método 2: Clonar este repositório

```bash
cd SillyTavern/public/scripts/extensions/
rm -rf SillyTavern-Chub-Search  # Remove a versão antiga
# Copie a pasta v3-chub-search-fixed para aqui e renomeie para SillyTavern-Chub-Search
```

## Diferenças entre as versões

### v1 (Original)
- Endpoint SillyTavern: `/api/content/import`
- Sem fallback para avatars
- Quebrou quando a API mudou

### v2 (Fixed by realcoloride - Abril 2024)
- Endpoint SillyTavern: `/api/content/importUUID`
- Adicionou fallback para avatars (mas como segunda opção)
- Quebrou novamente quando Cloudflare foi implementado (Setembro 2025)

### v4 (Esta versão - Novembro 2025)
- Endpoint SillyTavern: `/api/content/importUUID` (mantido)
- **Prioriza** o endpoint de avatars (primeira opção)
- Tratamento de erros robusto
- Mensagens informativas para o usuário
- Funciona mesmo com bloqueio Cloudflare

## Limitações Conhecidas

1. **API de busca ainda pode ser bloqueada**: A API de busca (`api.chub.ai/api/characters/search`) pode retornar 403. Neste caso, a extensão mostrará uma mensagem sugerindo acesso direto ao chub.ai.

2. **Imagens em WebP**: O endpoint de avatars retorna imagens em formato WebP ao invés do formato Tavern completo. Isso é suficiente para visualização, mas pode não incluir todos os metadados.

3. **Dependência do endpoint de avatars**: Se o `avatars.charhub.io` também for bloqueado no futuro, será necessária uma nova solução.

## Testado em

- SillyTavern 1.13.3 Release
- Novembro 2025

## Créditos

- **Versão Original**: [city-unit](https://github.com/city-unit/SillyTavern-Chub-Search)
- **v2 Fix**: [realcoloride](https://github.com/realcoloride/Fixed-SillyTavern-Chub-Search)
- **v3 Fix (2025)**: Análise e correção do problema de bloqueio Cloudflare

## Suporte

Se a extensão ainda não funcionar:

1. Verifique o console do navegador (F12) para mensagens de erro
2. Tente acessar https://chub.ai diretamente para verificar se o site está acessível
3. Verifique se você tem a versão mais recente do SillyTavern
4. Como última opção, acesse https://chub.ai/search diretamente no navegador

## Changelog

### v1.0.3 (Novembro 2025) - v4 Fix
- Prioriza endpoint de avatars que não está bloqueado\n- **Corrige erro 422** na API de busca, convertendo booleanos para 0/1 na URL
- Adiciona tratamento robusto de erros
- Melhora mensagens de erro para o usuário
- Adiciona logging detalhado
- Corrige problema de bloqueio Cloudflare

### v1.0.1 (Abril 2024) - v2 Fix
- Muda endpoint SillyTavern para `/api/content/importUUID`
- Adiciona fallback para endpoint de avatars
- Melhora lógica de paginação

### v1.0.0 (Original)
- Versão inicial
- Busca e download de personagens do Chub.ai
