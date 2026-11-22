# **Adaptação do LineageOS GSI para Samsung Galaxy A03 Core**

## 📱 Mantenedor não oficial: MarsXz
## GSI base por **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)**

---

## 🌍 Escolha o seu idioma
[English](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README.md) | [Español](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-ES.md) | **Português** | [Українська](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-UA.md) | [Русский](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-RU.md) | [Қазақша](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-KZ.md)

---

## 📺 Tutorial em vídeo

Se você não quiser ler as instruções, pode assistir a um **vídeo detalhado** sobre como instalar e adaptar a ROM.

**[Assistir ao guia completo no YouTube](https://youtu.be/JX0434AHy6A)**

---

Este projeto fornece uma **GSI AOSP/LineageOS** adaptada e otimizada especialmente para o **Samsung Galaxy A03 Core**.

É baseada no **LineageOS GSI do Andy Yan**, com ajustes extras para melhorar a compatibilidade, a estabilidade e a experiência geral do usuário.

---

## ⚠️ Aviso de isenção de responsabilidade

Você instala esta ROM **por sua conta e risco**.
**Não me responsabilizo** por bootloops, perda de dados, soft-bricks, danos ao aparelho ou qualquer outro prejuízo.

Se você não tem certeza do que está fazendo, **é melhor parar.**

---

## 📃 Documentação

Eu **não** sou desenvolvedor do LineageOS nem da GSI original.
Mantenho apenas esta **adaptação não oficial** feita especificamente para o Samsung Galaxy A03 Core.

As atualizações podem incluir:

- otimizações específicas para o dispositivo
- correções de compatibilidade de kernel e vendor
- meu módulo AIO adicional de otimização
- melhorias de estabilidade

---

## 💾 Guia de instalação (Flash)
[Clique aqui para ver o guia em Português](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/FLASHING_GUIDE_PT.md)

---

## 💾 Passos de adaptação
[Clique aqui para ver os passos em Português](https://github.com/MarsXZ-Official/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/system/ADAPTATION_GUIDE_PT.md)

---

## ⭐ Recursos

- Otimização para o **Samsung Galaxy A03 Core**
- Remoção de componentes desnecessários que não são usados neste aparelho
- Configurações ocultas do Treble para uma experiência de ROM mais completa
  Para ativar o Treble Settings:
  ```bash
  su -c "pm enable me.phh.treble.app/.TopLevelSettingsActivity"
  ```

* **O carregamento offline (com o aparelho desligado) reinicia o dispositivo automaticamente**
*(evita travamentos no logotipo da Samsung em alguns kernels)*

---

## ⛔ Problemas conhecidos

* **VoLTE não é suportado**
* **Exibição incorreta do nível de sinal da rede móvel**
*(mostra sempre 2 barras independentemente do sinal real)*

---

## 🔧 Correções futuras

O autor poderá lançar no futuro um **módulo Magisk** que potencialmente corrigirá parte desses problemas.

---

## 📌 Fontes e créditos

* **[LiteGApps](https://litegapps.github.io/)** — pacotes leves de GApps
* **[LineageOS](https://lineageos.org/)** — excelente ROM de código aberto
* **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)** — compilações do LineageOS GSI
