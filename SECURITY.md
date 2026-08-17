# Sécurité

## Bonnes pratiques implémentées

### 1. **Headers de sécurité** (netlify.toml)
- ✅ **Strict-Transport-Security (HSTS)**: Force HTTPS pendant 1 an
- ✅ **X-Frame-Options: DENY**: Empêche le clickjacking
- ✅ **X-Content-Type-Options: nosniff**: Prévient les attaques MIME sniffing
- ✅ **X-XSS-Protection**: Protection contre les XSS (navigateurs legacy)
- ✅ **Referrer-Policy**: Contrôle les informations de référent
- ✅ **Permissions-Policy**: Désactive les APIs dangereuses

### 2. **Content Security Policy (CSP)**
- ✅ Base restrictive: `default-src 'self'`
- ✅ Images: accepte uniquement self, data, blob et HTTPS
- ✅ Scripts: self + CDNs de confiance (Tailwind, Babel, React)
- ✅ Styles: self + CDNs de confiance
- ✅ WebSockets: uniquement vers Supabase
- ✅ `block-all-mixed-content`: Force HTTPS partout
- ⚠️ `'unsafe-inline'` et `'unsafe-eval'`: Nécessaires pour Babel (JSX inline)

### 3. **Variables d'environnement sensibles**
- ✅ `.env.example` fourni comme template
- ✅ **IMPORTANT**: `.env` est dans `.gitignore` - jamais commiter les vrais secrets!
- ✅ Supabase URL en dur est OK (c'est une clé anon, pas un secret)

### 4. **Authentification & Données**
- ✅ Supabase gère l'authentification (tokens JWT)
- ✅ RLS (Row Level Security) sur les tables Supabase
- ⚠️ À vérifier: Les tokens sont stockés dans localStorage - vulnerable aux XSS

### 5. **Dépendances**
- React, ReactDOM, Babel: Chargés via CDN (Cloudflare)
- ✅ Versions pinées pour reproductibilité
- ⚠️ À faire: Auditer les dépendances régulièrement avec `npm audit`

## Recommandations supplémentaires

### 🔒 À FAIRE MAINTENANT:
1. **Activer HTTPS/SSL** - Netlify le fait automatiquement ✅
2. **Activer les Fonction Edge** pour rate-limiting (Netlify Pro)
3. **Ajouter une Protection CORS** si nécessaire
4. **Audit des dépendances**: `npm audit`

### ⚠️ À AMÉLIORER:
1. **Remplacer localStorage par sessionStorage** pour les tokens sensibles
2. **Implémenter une protection CSRF** pour les formulaires
3. **Activer 2FA** sur Supabase
4. **Monitorer les logs** Supabase pour activités suspectes
5. **Établir des SOP (Subresource Integrity)** pour les CDN

### 🛡️ À SURVEILLER:
- Mises à jour de sécurité React/Babel
- Alertes de vulnérabilités npm
- Tentatives d'accès non autorisées Supabase

## Tests de sécurité

Vérifie les en-têtes de sécurité:
```bash
# Via curl
curl -I https://ton-domaine.netlify.app | grep -E "Strict-Transport|X-Frame|Content-Security"
```

## Signaler une vulnérabilité

Si tu découvres une faille de sécurité, contacte le mainteneur directement.
Ne pas publier les vulnérabilités publiquement.
