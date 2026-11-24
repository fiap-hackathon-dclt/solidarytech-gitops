# solidarytech-gitops

Estado desejado do cluster da **SolidaryTech**. O Argo CD observa este repositório e sincroniza o cluster com ele.

É a **única via de deploy**. Ninguém roda `kubectl apply` na própria máquina: para mudar o que está no cluster, muda-se este repositório, e o histórico de commits é o registro auditável do que foi promovido, quando e por qual build.

## O que vai morar aqui

| Peça | Conteúdo |
|---|---|
| **Microsserviços** | Um chart por serviço (`ngo-service`, `donation-service`, `volunteer-service`), com **requests e limits** de CPU e memória ajustados pelo consumo medido |
| **Observabilidade** | Collector OpenTelemetry, Prometheus, Grafana e Loki |
| **Dashboards** | O dashboard SRE, com SLIs, SLOs e consumo do error budget do `donation-service` |
| **Applications** | As Applications do Argo CD que apontam para cada chart |

## Como uma versão chega ao cluster

```
merge na main de solidarytech-microservices
  → CI testa, varre e publica a imagem no ECR, com o SHA do commit como tag
  → CD grava a nova tag no values do serviço, aqui
  → Argo CD detecta a diferença e sincroniza
  → nova versão no cluster
```

## Rightsizing

Requests e limits não são chutados: partem de um valor inicial conservador, são medidos sob carga e ajustados aqui, por commit. O enunciado exige que o ajuste chegue ao cluster pelo fluxo GitOps, então o antes e o depois ficam no histórico deste repositório.

## Estado

Repositório recém-criado, ainda sem charts.
