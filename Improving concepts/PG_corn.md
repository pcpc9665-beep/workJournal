
your-project/
│
├── supabase/
│   ├── config.toml                  ← add the schedule config here (if using Scheduled Functions)
│   ├── seed.sql
│   │
│   ├── migrations/                  ← your SQL migrations (schema, functions, cron jobs)
│   │   ├── 20250101000000_create_products_table.sql
│   │   ├── 20250101000001_create_reservations_table.sql
│   │   ├── 20250101000002_reserve_stock_function.sql
│   │   └── 20250101000003_decrement_reserved_stock_function.sql
│   │
│   └── functions/                   ← all Edge Functions go here
│       ├── _shared/                 ← optional: shared helpers/types across functions
│       │   └── supabaseClient.ts
│       │
│       └── release-expired-reservations/
│           └── index.ts             ← the TS function you just got
│
├── .env                              ← local env vars (SUPABASE_URL, SUPABASE_SERVICE_ROLE_KEY)
├── package.json
└── ...
