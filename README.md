# Satisfeito

[English](README.en.md) | **Português**

> Pesquisa de satisfação simples e rápida para negócios locais: restaurantes, lanchonetes e lojas.

Este repositório é a **vitrine** do projeto: mostra o que o Satisfeito faz, as telas e as decisões técnicas. O código do sistema fica em um repositório privado.

**Demonstração online:** https://satisfeito.vercel.app (o cadastro de empresas é por convite)

---

## O que é

O cliente escaneia um **QR code** na mesa, no balcão ou na nota fiscal, responde em menos de 30 segundos pelo celular e não precisa criar conta. O dono do negócio acompanha tudo num **painel** com médias, gráficos e um **relatório escrito por IA**, pronto para copiar e mandar no WhatsApp da equipe.

Projeto feito por um estudante de Ciência da Computação, em uso real no primeiro negócio (uma lanchonete) e pensado para ser oferecido a outros.

## Telas

**Pesquisa que o cliente responde pelo celular** (abre pelo QR code, sem login). Nota de 1 a 10 e, se a nota for baixa, uma caixa opcional "Onde podemos melhorar?":

![Pesquisa pública](prints/pesquisa-publica.jpg)

**Painel da empresa**

| Início | Pesquisas |
|---|---|
| ![Início do painel](prints/inicio.jpg) | ![Lista de pesquisas](prints/painel-pesquisas.jpg) |

| Link e QR code | Editor de perguntas |
|---|---|
| ![Link, QR code e cartaz](prints/painel-link-qr.jpg) | ![Editor de perguntas](prints/painel-perguntas.jpg) |

**Resultados**: período de 7, 30 ou 90 dias, comparação com o período anterior, NPS, distribuição das notas, dia da semana e turno:

![Resultados](prints/dashboard-topo.jpg)

![Nota por dia da semana e turno](prints/dashboard-dia-turno.jpg)

**Respostas**, com selo de suspeita e opção de descartar:

![Lista de respostas](prints/respostas.jpg)

**Relatório para a equipe** (texto pronto para o WhatsApp):

![Relatório para a equipe](prints/relatorio-equipe.jpg)

> Os números dos prints são de exemplo, criados só para a demonstração, e o texto da IA no relatório foi simulado.

## Tecnologias

| Camada | Tecnologia |
|---|---|
| Framework | Next.js 16 (App Router) + React 19 |
| Linguagem | TypeScript |
| Estilo | Tailwind CSS 4 |
| Banco e login | Supabase (Postgres + Auth) com RLS |
| Gráficos | Recharts |
| Anti-robô | Cloudflare Turnstile |
| IA | API da Anthropic (Claude), chamada só pelo servidor |
| Hospedagem e CI | Vercel e GitHub Actions |

## Decisões técnicas que eu quero destacar

- **Isolamento de dados no banco.** Cada empresa só lê e edita os próprios dados por regras RLS no Postgres, e há testes SQL automáticos no CI que provam isso, inclusive para as tabelas só do servidor.
- **Proteção contra respostas falsas sem punir clientes reais.** Marca no navegador, tempo mínimo de resposta, Turnstile e um painel que marca rajadas de notas ruins como suspeitas para o dono descartar. Decidi **não usar IP**: o Wi-Fi do estabelecimento é compartilhado e bloquearia clientes de verdade. O Turnstile falha fechando: sem token válido, a resposta não é gravada.
- **IA com rédea curta.** O texto do cliente é tratado como dado, nunca como instrução. A IA escreve só a prosa; os números do relatório vêm do banco, e o servidor recusa o texto se a IA inventar um número. Há teto de uso por dono e reserva da vaga no banco antes de chamar a API.
- **Cadastro por convite de uso único.** O dono gera um link com validade; o banco guarda só o hash do código e o servidor "gasta" o convite com um único `UPDATE` condicional, então o mesmo link não serve para duas contas.
- **Acessibilidade e mobile-first.** Alvos de toque de 44 px, contraste conferido, foco e leitor de tela testados, auditoria com axe-core, interface em português e sem emojis (ícones SVG próprios).
- **Direito de exclusão (LGPD).** O dono apaga a conta, as empresas, as pesquisas e todas as respostas pelo próprio painel.

## Quer ver o código?

O código fica em um repositório privado. Se você é recrutador ou quer conversar sobre o projeto, fale comigo pelo meu perfil do GitHub: [@CaioPortela17](https://github.com/CaioPortela17).

## Direitos

© 2026 Caio Portela. Todos os direitos reservados. Os textos e as imagens deste repositório não podem ser copiados ou reutilizados sem autorização.
