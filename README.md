# A Mina das Milhas — Landing Pages

Repositório com as páginas de venda do Sistema M.I.N.A.

## Estrutura de URLs

Cada pasta com `index.html` gera uma URL automática no GitHub Pages:

- `/sistema/index.html` → `vsl.amindasmilhas.com.br/sistema`
- `/sem-delay/index.html` → `vsl.amindasmilhas.com.br/sem-delay`
- `/outra-pagina/index.html` → `vsl.amindasmilhas.com.br/outra-pagina`

## Como Adicionar Uma Nova Página

1. **Crie uma nova pasta** com o nome do slug desejado:
   ```bash
   mkdir nova-pagina
   ```

2. **Copie o arquivo `index.html`** de uma página existente (ou crie um novo):
   ```bash
   cp sistema/index.html nova-pagina/index.html
   ```

3. **Copie a pasta `images`** se precisar de imagens específicas:
   ```bash
   cp -r sistema/images nova-pagina/
   ```

4. **Customize o arquivo `index.html`** conforme necessário:
   - Altere o `<title>`
   - Modifique o conteúdo
   - Ajuste cores, textos, CTAs

5. **Commit e push** para o repositório:
   ```bash
   git add nova-pagina/
   git commit -m "adiciona página: nova-pagina"
   git push
   ```

## Variações Recomendadas

### `/sistema` (Versão Padrão)
- Conteúdo oculto até o final do vídeo
- Foco em conversão durante VSL

### `/sem-delay` (Sem Delay)
- Todo conteúdo visível desde o início
- Ideal para tráfego já aquecido ou retargeting

### Possíveis Novas Páginas
- `/mobile` — Otimizada para conversão mobile
- `/black-friday` — Versão com desconto especial
- `/referral` — Landing para afiliados/referência
- `/webinar` — Página de apresentação
- `/waitlist` — Página de pré-lançamento

## Especificações das Imagens

Todas as imagens devem estar em formato `.webp` para otimização:

- **Depoimentos**: `images/depoimento X.webp` (recomendado: 1080x1350px)
- **Foto da Mentora**: `images/foto oficial da natália.webp` (recomendado: 600x800px)

## GitHub Pages Setup

1. Vá para **Settings → Pages**
2. Selecione a branch `main` como source
3. Configure o domínio customizado: `vsl.amindasmilhas.com.br`

Após isso, o site será publicado automaticamente a cada push.

## Notas de Manutenção

- **Cada página é independente**: alterações em uma não afetam as outras
- **CSS duplicado é intencional**: permite customizações por página
- **Imagens**: mantenha na pasta `images/` de cada página para não quebrar links
- **Links de pagamento**: atualize os links do Hotmart em cada página

## Troubleshooting

### Imagens não aparecem
Verifique se o caminho está correto:
```html
<!-- ✅ Correto -->
<img src="images/depoimento 1.webp" alt="...">

<!-- ❌ Errado -->
<img src="../images/depoimento 1.webp" alt="...">
```

### Página não aparece no GitHub Pages
- Espere 2-3 minutos após o push
- Verifique se há um `index.html` na pasta
- Confirme que a pasta está no repositório remoto (`git push`)

### Vídeo VTurb não carrega
Verifique o ID do player na tag `<vturb-smartplayer>` e no script.
