# Direção de integração com IA

O OrbitSEC Copilot será uma camada opcional de explicação para laboratórios autorizados. Seu objetivo é ajudar o usuário a interpretar observações estruturadas sem transformar respostas do modelo em fatos ou comandos confiáveis.

## Princípios

- Preferência por inferência local em projetos privados.
- Contexto mínimo, estruturado e aprovado pelo usuário.
- Citação das observações utilizadas em cada explicação.
- Separação entre observação, hipótese, CVE candidata e achado.
- Nenhuma execução automática de terminal.
- Provedores remotos opcionais e acionados explicitamente.

## Primeiros casos de uso

1. Explicar um host ou serviço selecionado.
2. Resumir notas aprovadas pelo usuário.
3. Sugerir perguntas para validação manual.
4. Produzir rascunhos rastreáveis para revisão.

## Avaliação

O protótipo deverá medir afirmações sem suporte, confusão entre observação e achado, desvio de escopo e exposição desnecessária de contexto. Respostas do modelo nunca substituirão evidência validada.
