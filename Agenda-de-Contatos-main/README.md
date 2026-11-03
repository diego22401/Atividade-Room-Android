# Agenda de Contatos — Room + RandomUser API

Projeto desenvolvido para a missão de evolução do aplicativo anterior.

## Opção escolhida
**Agenda de Contatos → RandomUser API**.

API usada: `https://randomuser.me/`

## Arquitetura

```text
RandomUser API
      ↓
   Repository
      ↓
      Room
      ↓
     Flow
      ↓
    ViewModel
      ↓
   StateFlow
      ↓
      UI
```

O **Room é a fonte única de verdade dos dados exibidos pela interface**. O Repository consulta a API e grava os dados no Room. A UI observa somente o fluxo do banco.

## Requisitos da missão atendidos

- Migração para Room com `@Entity`, `@Dao` e `@Database`.
- Repository como Single Source of Truth.
- `Flow` de contatos vindo do Room.
- `AndroidViewModel` coletando o Flow como `StateFlow`.
- Estado de sincronização separado dos dados: `Idle`, `Loading` e `Error`.
- Estratégia offline-first.
- Erro de rede não apaga a lista já salva.
- Botão **Atualizar** para nova sincronização.

## Offline-first

Ao iniciar, o ViewModel observa imediatamente o Room e inicia uma sincronização em segundo plano. Se a API estiver indisponível, o Room continua entregando os últimos contatos salvos e a interface apresenta um aviso.

Os dados existentes só são substituídos depois que a API retorna uma resposta válida.

## Estrutura principal

```text
app/src/main/java/com/davisena/agendacontatos/
├── data/
│   ├── local/
│   │   ├── AgendaDatabase.kt
│   │   ├── ContatoDao.kt
│   │   └── ContatoEntity.kt
│   ├── remote/
│   │   ├── RandomUserService.kt
│   │   └── RetrofitProvider.kt
│   └── repository/
│       └── ContatoRepository.kt
├── model/
│   └── Contato.kt
├── ui/theme/
├── viewmodel/
│   └── AgendaViewModel.kt
└── MainActivity.kt
```
