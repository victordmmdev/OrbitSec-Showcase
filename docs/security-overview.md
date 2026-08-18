# Visão geral de segurança

O OrbitSEC trata saída de ferramentas, arquivos importados e conteúdo de projetos como dados não confiáveis. A arquitetura separa validação, rastreabilidade, persistência e sessão para reduzir acoplamento e facilitar revisão.

## Limites principais

- Integrações ativas respeitam o escopo autorizado declarado.
- Parsers possuem limites de tamanho e complexidade.
- Observações preservam sua origem e identidade de projeto.
- Material de acesso não é armazenado junto do conteúdo protegido.
- Alterações em conteúdo persistido devem falhar de forma segura.
- Estado sensível é reduzido quando a sessão termina.

## O que esta documentação não publica

- Parâmetros criptográficos ou formatos internos.
- Chaves, wrappers, identificadores ou fluxos de recuperação.
- Código de autenticação, proteção ou persistência.
- Scans, alvos, flags, credenciais, transcrições ou bancos reais.

## Limites das alegações

Testes automatizados verificam invariantes selecionados, mas o projeto ainda não passou por auditoria profissional independente. Esta documentação descreve objetivos e comportamentos observados, não uma garantia de adequação para produção.
