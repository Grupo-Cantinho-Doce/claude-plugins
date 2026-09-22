# cantinho-tools — marketplace de plugins do Claude Code

Marketplace interno do Grupo Cantinho Doce para distribuir plugins do Claude Code
para todos os assentos do plano **Team**.

## Plugins

| Plugin | O que é | Origem / Licença |
|---|---|---|
| **security-audit** | Auditoria de segurança orientada a fonte (recon → hunting → validação → verificação → relatório). É **guidance por padrão**: só roda o fluxo completo de 6 fases quando pedido explicitamente ("audite este código", "pentest"). | [Cloudflare](https://github.com/cloudflare/security-audit-skill) · MIT |

## Como publicar este repositório

1. Crie um repositório no GitHub **da sua organização** (privado, recomendado),
   por exemplo `sua-org/claude-plugins`.
2. Suba o conteúdo desta pasta para lá:
   ```
   git init
   git add .
   git commit -m "marketplace interno: security-audit (Cloudflare)"
   git branch -M main
   git remote add origin git@github.com:sua-org/claude-plugins.git
   git push -u origin main
   ```

## Como habilitar para TODOS os assentos (plano Team)

Como **Owner** da organização, em **claude.ai → Admin Settings → Claude Code →
Managed settings**, cole (trocando `sua-org/claude-plugins` pelo repo real):

```json
{
  "extraKnownMarketplaces": {
    "cantinho-tools": {
      "source": { "source": "github", "repo": "sua-org/claude-plugins" }
    }
  },
  "enabledPlugins": {
    "security-audit@cantinho-tools": true
  }
}
```

- `extraKnownMarketplaces` registra este repositório como marketplace.
- `enabledPlugins` **força** o plugin para todos — cada membro recebe no próximo
  login, sem precisar instalar nada.

## Como um membro usa

Depois de logado (o plugin aparece em `/plugin list` como origem `managed`):

```
/security-audit:security-audit <caminho ou alvo>
```

ou, em linguagem natural, algo como "faça uma auditoria de segurança neste
repositório".

## Atenção (antes de forçar para todos)

- É **código de terceiro** que roda com as permissões do agente e **lê o seu
  código**; no fluxo completo ele executa validadores em Node (`.cjs`).
- A Cloudflare é fonte séria, mas **teste em um assento primeiro** e só então
  habilite via `enabledPlugins` para toda a organização.
- Versione (`version` em SemVer) — o Claude só oferece atualização quando a
  versão sobe.

---

A skill em `plugins/security-audit/skills/security-audit/` é da Cloudflare,
distribuída sob a licença MIT (ver `plugins/security-audit/LICENSE`). Este
repositório apenas a **empacota como plugin** para distribuição interna; o
conteúdo da skill não foi alterado.
