# Advanced RBAC & Audit System

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green.svg)](https://fastapi.tiangolo.com)

**Repo:** [https://github.com/Omimas/advanced-rbac-audit-system/](https://github.com/Omimas/advanced-rbac-audit-system/)

## Basit Özet
FastAPI kullanılarak geliştirilmiş, kurumsal düzeyde Hiyerarşik Rol Tabanlı Erişim Kontrolü (RBAC) ve kapsamlı denetim (audit) altyapısı sunan bir kimlik doğrulama ve yetkilendirme servisidir. Kullanıcı işlemleri atomik olarak loglanır, JWT tabanlı kimlik denetimi token kara liste (blacklist) mekanizmasıyla güçlendirilmiştir.

---

## Teknik Açıklama: Mimari ve Yaklaşım

Sistem üç ana güvenlik motoru üzerine inşa edilmiştir:

1. **RBAC Engine (Rol ve İzin Yönetimi):**
   - **Hiyerarşik Roller:** Roller birbirinden miras alabilir (`parent_id`). Örneğin, `admin` rolü `manager` yetkilerini kapsar.
   - **Granüler Yetkilendirme:** İzinler `resource:action` formatında tanımlanır. Wildcard (`*:*` veya `users:*`) desteği ile dinamik yetki kontrolü sağlanır.
   - **Geçici Roller:** Kullanıcılara belirli bir süre için (`expires_at`) rol atanabilir.

2. **Audit Engine (Denetim Kayıtları):**
   - Sistemdeki tüm kritik state (durum) değişiklikleri (CREATE, UPDATE, DELETE, LOGIN) veritabanına atomik olarak kaydedilir.
   - Loglar; IP adresi, User-Agent, Session ID, işlem öncesi/sonrası JSON state'leri (`before_state`, `after_state`) ve hata detaylarını içerir.
   - Hukuki ve operasyonel takip için silinmiş kullanıcıların logları (Snapshot) korunur.

3. **Güvenlik ve Auth Mekanizması:**
   - Access & Refresh JWT mimarisi kullanılmıştır.
   - **Token Revocation:** Çıkış yapıldığında veya token yenilendiğinde eski tokenlar `token_blacklist` tablosuna eklenir.
   - **Brute-Force Koruması:** 5 ardışık hatalı girişte hesap otomatik olarak kilitlenir (`is_locked=True`).

---

## Kurulum ve Çalıştırma

### 1. Bağımlılıkların Yüklenmesi
Sistemi sanal bir ortamda (virtual environment) çalıştırmanız önerilir.

pip install fastapi uvicorn sqlalchemy pydantic python-jose passlib bcrypt python-multipart
python rbac_audit_system.py
# → http://localhost:8000/docs
# → admin / Admin@123!

```bash
pip install fastapi uvicorn sqlalchemy pydantic python-jose passlib bcrypt python-multipart



python rbac_audit_system.py
