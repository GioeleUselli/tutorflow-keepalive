# TutorFlow keep-alive

Repository pubblico con un solo scopo: **tenere sveglia** l'app TutorFlow su
Azure App Service (piano Free F1), che altrimenti viene sospesa dopo circa
20 minuti di inattività.

Ogni 5 minuti un workflow GitHub Actions chiama l'endpoint pubblico:

```
https://tutor-flow-cwfverfpb9b3bub0.italynorth-01.azurewebsites.net/health
```

Non contiene codice dell'applicazione né credenziali: solo l'URL pubblico di
health check. Per questo può stare in un repository pubblico, dove le GitHub
Actions sono gratuite e illimitate.

## Note

- Il piano Free di Azure non supporta "Always On": questo ping evita la
  sospensione per inattività, ma Azure può comunque riciclare l'istanza per
  manutenzione. In quel caso i promemoria in sospeso partono alla prima
  richiesta.
- GitHub può ritardare i workflow schedulati di qualche minuto nei momenti di
  carico: con un intervallo di 5 minuti e un timeout di inattività di 20 minuti
  c'è margine sufficiente.
- GitHub disattiva i workflow schedulati dopo 60 giorni senza attività nel
  repository: basta un commit (o riattivarlo da Actions) per ripartire.

