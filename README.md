# LilyCrafts — Integrated E-Commerce + Admin Panel

A unified React/TypeScript/Vite project combining the LilyCrafts storefront with a fully functional Supabase-backed admin panel.

---

## 🗂 Project Structure

```
src/
├── admin/
│   ├── components/
│   │   ├── AdminLayout.tsx    ← Dark sidebar shell
│   │   └── AdminRoute.tsx     ← Auth guard
│   ├── hooks/
│   │   ├── useProducts.ts     ← CRUD + image upload
│   │   └── useOrders.ts       ← Orders + status update
│   └── pages/
│       ├── AdminLogin.tsx
│       ├── AdminDashboard.tsx
│       ├── AdminProducts.tsx
│       ├── AdminOrders.tsx
│       ├── AdminCustomers.tsx
│       └── AdminSettings.tsx
├── context/
│   ├── AuthContext.tsx        ← Supabase auth state
│   ├── CartContext.tsx
│   └── WishlistContext.tsx
├── lib/
│   ├── supabase.ts            ← Client + DB types
│   └── utils.ts
└── pages/ (public store pages)
```

---

## ⚙️ One-time Setup

### 1. Install dependencies
```bash
npm install
```

### 2. Configure environment variables
```bash
cp .env.example .env.local
```
Edit `.env.local`:
```env
VITE_SUPABASE_URL=https://your-project-id.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key-here
```
Find these in: **Supabase Dashboard → Project Settings → API**.

### 3. Run the SQL schema
1. Open Supabase → **SQL Editor**
2. Paste the entire contents of `supabase-setup.sql`
3. Click **Run**

Creates: `profiles`, `products`, `orders`, `order_items` tables, all RLS policies, `product-images` Storage bucket, and 10 sample products.

### 4. Create your first admin user
1. `npm run dev` → go to `/admin/login` → **sign up**
2. In Supabase SQL Editor run:
```sql
UPDATE public.profiles SET role = 'admin' WHERE email = 'your-email@example.com';
```
3. Sign in again → redirected to dashboard ✓

---

## 🚀 Development
```bash
npm run dev      # http://localhost:8080
npm run build
npm run preview
```

---

## 🔐 Auth Flow
- Unauthenticated → `/admin/*` redirects to `/admin/login`
- Authenticated non-admin → redirects to `/`
- Session persists across refreshes via Supabase JWT

---

## 🗄 Database Schema

| Table | Key columns |
|-------|------------|
| `profiles` | `id` (FK auth.users), `email`, `full_name`, `role`, `is_active` |
| `products` | `id`, `name`, `description`, `price`, `category`, `stock`, `image_url` |
| `orders` | `id`, `user_id`, `status`, `total_amount`, `shipping_*`, `notes` |
| `order_items` | `id`, `order_id`, `product_id`, `quantity`, `unit_price` |

---

## 📍 Routes

| Path | Auth |
|------|------|
| `/`, `/shop`, `/product/:id`, `/checkout` | Public |
| `/admin/login` | Public |
| `/admin`, `/admin/products`, `/admin/orders`, `/admin/customers`, `/admin/settings` | Admin only |
