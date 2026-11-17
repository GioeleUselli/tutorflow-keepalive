# TutorFlow keep-alive (riserva)

Repository pubblico con un solo scopo: dare un ping di **riserva** all'app
TutorFlow su Azure App Service (piano Free F1), che altrimenti viene sospesa
dopo circa 20 minuti di inattività.

Un workflow GitHub Actions chiama l'endpoint pubblico:

```
https://tutor-flow-cwfverfpb9b3bub0.italynorth-01.azurewebsites.net/health
```

Non contiene codice dell'applicazione né credenziali: solo l'URL pubblico di
health check.

> **Nota importante — il cron di GitHub Actions non è affidabile.**
> Da fine 2026 i workflow `schedule` partono con ore di ritardo o non partono
> affatto; GitHub lo documenta come "best-effort", soggetto a ritardi e
> esecuzioni scartate. Per questo il keep-alive vero e proprio è una **Logic App
> di Azure** (`tutorflow-keepalive` nel resource group `tutorflow-rg`) che chiama
> lo stesso `/health` ogni 5 minuti con puntualità.
>
> Questo repository resta attivo solo come ping aggiuntivo quando il cron di
> GitHub decide di eseguire.

## Spegnere questo ping

`Settings → Actions → General`, oppure disattivare il workflow:

```powershell
gh workflow disable keepalive.yml --repo GioeleUselli/tutorflow-keepalive
```

GitHub disattiva comunque da solo i workflow schedulati dopo 60 giorni senza
attività nel repository.
