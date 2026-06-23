# Environment & Configuration (Apps)

- Never commit secrets. Configuration comes from environment variables; `.env`
  is git-ignored.
- Document every required variable in `.env.example`.
- Read configuration through `config()` keys, not `env()` calls outside config
  files — config is cached in production and `env()` returns null there.
