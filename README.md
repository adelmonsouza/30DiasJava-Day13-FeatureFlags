# Day 13/30 — Feature Flags & Experimentation Platform

> **LaunchDarkly + Unleash, experimentos A/B, canary rollouts e feature toggles no runtime.**

## 📚 Overview

Este projeto implementa uma **Feature Flags & Experimentation Platform** que permite rollouts progressivos, testes A/B e kill switches instantâneos para qualquer microserviço Spring Boot.

## 🚩 Features

- **Feature flag SDK** (`feature-flags-starter`) com suporte a LaunchDarkly e Unleash (modo client-side e server-side)
- **Canary rollouts** graduais por percentual de tráfego (ex.: 10% → 50% → 100%) com rollback automático via health checks
- **A/B testing framework** com segmentação por usuário (`userId`), tenant, país e atributos customizados
- **Kill switches** operacionais: desliga features instantaneamente via dashboard sem tocar no código
- **Feature analytics**: dashboard mostra adoção, impacto em métricas (revenue, error rate) e recomendações de rollout

## 💡 Why it matters

- Produtos lançam features com confiança: rollback em segundos se algo quebrar
- Experimentação viabiliza decisões baseadas em dados — não opiniões
- Devs reduzem branches longas: features ficam mergidas com flags `disabled` até o momento certo

## 🚀 Quick Start

### Prerequisites

- Java 21+
- Maven 3.8+
- LaunchDarkly account ou Unleash server

### Installation

```bash
# Clone o repositório
git clone https://github.com/adelmonsouza/30DiasJava-Day13-FeatureFlags.git
cd 30DiasJava-Day13-FeatureFlags

# Build
mvn clean install

# Incluir no seu projeto Spring Boot
<dependency>
    <groupId>com.adelmonsouza</groupId>
    <artifactId>feature-flags-starter</artifactId>
    <version>1.0.0</version>
</dependency>
```

### Configuration

```yaml
# application.yml
feature-flags:
  provider: launchdarkly # ou unleash
  sdk-key: ${LAUNCHDARKLY_SDK_KEY}
  offline-mode: false
  cache-ttl: 30s
```

### Usage

```java
@Service
public class CheckoutService {
    
    @FeatureFlag("new-checkout")
    public void processCheckout(Order order) {
        // Nova implementação
    }
    
    @FeatureFlag(value = "old-checkout", enabled = false)
    public void processCheckoutLegacy(Order order) {
        // Implementação antiga (fallback)
    }
}
```

## 🧩 Implementation Notes

- **Client-side evaluation**: flags resolvidas no pod, sem latência extra por chamada externa
- **Server-side SDK**: Spring Boot `@FeatureFlag("new-checkout")` com fallback para default quando o serviço de flags está offline
- **Targeting rules**: YAML/JSON define segmentos (`beta-users`, `eu-region`, `tier=premium`) e porcentagens
- **Event tracking**: cada avaliação de flag envia evento para analytics (LaunchDarkly insights ou Prometheus metrics)
- **Integrations**: Webhooks para Slack/Teams quando flags mudam de estado, e APIs REST para gerenciar flags via GitOps

## ✅ Daily checklist

- SDK conectado ao serviço de flags (LaunchDarkly/Unleash) e health check `feature-flags.connected` = `true`
- Rollouts graduais monitorados via dashboards (adoption rate, error rate delta)
- Kill switches testados em staging antes de produção

## 📚 Documentation

- **Blog post**: https://enouveau.io/blog/2025/11/13/feature-flags-experimentation.html
- **Series**: [#30DiasJava](https://github.com/adelmonsouza/30DiasJava)

## 📄 License

MIT License

## 👤 Author

**Adelmon Souza**

- GitHub: [@adelmonsouza](https://github.com/adelmonsouza)
- LinkedIn: [in/adelmonsouza](https://linkedin.com/in/adelmonsouza)

---

**Next episode** → Day 14/30 — API Gateway & Rate Limiting (em breve)
