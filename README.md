# Terraform na Azure — szablon kursanta

Szablon do szkolenia „Terraform + Claude Code na Azure”. Każdy kursant pracuje w jednej,
już istniejącej grupie zasobów z uprawnieniami Contributor.

- **[INSTRUKCJE.md](INSTRUKCJE.md)**: instrukcja dla kursanta (setup, zasady demo, typowe błędy).
- **[CLAUDE.md](CLAUDE.md)**: zasady i kontekst dla Claude Code.

## Szybki start

```bash
source ~/.tf-training/studentNN.env   # w każdej nowej sesji terminala
./scripts/login.sh                    # opcjonalnie: az login (do `az resource list`)
terraform init
terraform plan
terraform apply                       # bez -auto-approve
```

## Struktura

| Plik | Zawartość |
|------|-----------|
| `main.tf` | `data` grupy zasobów i output |
| `network.tf` | prywatna strefa DNS |
| `security.tf` | `random_string.suffix` do unikalnych nazw |
| `monitoring.tf` | action group (e-mail) |
| `variables.tf`, `outputs.tf`, `providers.tf` | zmienne, outputy, konfiguracja providera |
| `vm.tf.disabled` | ćwiczenie: VM nginx `Standard_B1s` (włącz przez usunięcie `.disabled`) |
| `storage.tf.disabled` | ćwiczenie: storage account |
| `scripts/` | `login.sh` (logowanie `az` jako service principal) |
| `.claude/` | ustawienia, hooki, `/plan-review`, agent `tf-reviewer` |
| `.github/workflows/` | CI (fmt, validate, trivy) i CD (ręczny plan/apply) |

## Najważniejsze zasady

- Maksymalnie **1 VM** (`Standard_B1s`) naraz. Zanim postawisz kolejną, zrób `terraform destroy`.
- Region tylko z `var.location`, tagi z grupy zasobów.
- Część usług (Firewall, Bastion, AKS, Cosmos DB, …) blokuje polityka. Pełna lista jest w [INSTRUKCJE.md](INSTRUKCJE.md).
- Nie commituj plików `.env` ani `*.tfvars`. `terraform.tfstate` jest w repo, bo commituje go job CD.

