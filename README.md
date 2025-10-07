# ModelCheck World of Blocks

## 👥 Equipe

- ANDRÉ MALMSTEEN OLIVEIRA AMORIM
- ALEXANDRE PEREIRA DE SOUZA JUNIOR
- BENJAMIM ISAAC RIBEIRO LIMA
- DIEGO GABRIEL SILVA AZEVEDO
- GUILHERME DA SILVA PEREIRA
- JOÃO PEDRO CASTRO DAS VIRGENS
- LEONARDO BRANDÃO DO AMARANTE
- MANFRED LIMA VEIGA
- MATEUS CAVALCANTE
- VITHOR JUNIOR DA ENCARNAÇÃO VITORIO

## 📋 Descrição

Projeto de verificação formal usando **NuSMV** para resolver o problema do mundo dos blocos com dimensões heterogêneas. Utiliza a metodologia **"Planning as Model Checking"** onde objetivos são verificados através da geração de contraexemplos.

## 🧩 Características

- **Blocos com tamanhos diferentes**: a=1, b=1, c=2, d=3
- **Sistema de coordenadas**: Campo 6x4 (posições 0-5, altura 0-3)
- **Sem restrições de estabilidade**: Qualquer bloco sobre qualquer outro
- **Lógica invertida**: `!EF(goal)` - "É impossível alcançar o objetivo?"

## ✅ Resultados das 3 Situações

### 🎯 Situação 1 - Múltiplos Objetivos

**Estado Inicial**: a:(3,0), b:(5,0), c:(0,0), d:(3,1)

| Goal    | Configuração                | Passos | Status |
| ------- | --------------------------- | ------ | ------ |
| **SF1** | Torre dupla com c no topo   | 7      | ✅     |
| **SF2** | Configuração escalonada     | 7      | ✅     |
| **SF3** | Distribuição linear         | 3      | ✅ ⚡  |
| **SF4** | a sobre c, demais dispersos | 5      | ✅     |

### 🎯 Situação 2 - Reorganização Completa

**Estado Inicial**: a:(0,1), b:(1,1), c:(0,0), d:(3,0)  
**Estado Final**: a:(3,2), b:(4,2), c:(3,1), d:(3,0)  
**Resultado**: ✅ **5 passos** (mais eficiente que sequência manual)

### 🎯 Situação 3 - Migração de Torres

**Estado Inicial**: a:(3,0), b:(5,0), c:(0,0), d:(3,1)  
**Estado Final**: a:(0,1), b:(1,1), c:(0,0), d:(3,0)  
**Resultado**: ✅ **6 passos**

## 🚀 Execução

```bash
# Situação 1 (editar para ativar objetivo desejado)
NuSMV situacao1.smv

# Situação 2
NuSMV situacao2.smv

# Situação 3
NuSMV situacao3.smv
```

## 📊 Resultados Finais

- **Taxa de sucesso**: 100% (7/7 objetivos)
- **Mais eficiente**: Situação 1 SF3 (3 passos)
- **Metodologia**: Planning as Model Checking validada
- **Verificação formal**: Todos objetivos comprovadamente alcançáveis

---

_Fundamentos de IA_
