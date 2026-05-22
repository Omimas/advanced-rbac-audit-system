# Advanced RBAC & Audit System

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green.svg)](https://fastapi.tiangolo.com)

**Repo:** https://github.com/Omimas/advanced-rbac-audit-system/

## Simple Summary

An enterprise-grade authentication and authorization service built with FastAPI, providing Hierarchical Role-Based Access Control (RBAC) and a comprehensive audit infrastructure. User operations are logged atomically, and JWT-based authentication is fortified with a token blacklist mechanism to ensure strict access control.

---

## Technical Explanation: Architecture & Approach

The system is built upon three primary security engines:

### 1. RBAC Engine (Role and Permission Management)

- **Hierarchical Roles:** Roles can inherit from one another (`parent_id`). For instance, the `admin` role automatically encompasses `manager` permissions.
- **Granular Authorization:** Permissions are defined in a `resource:action` format. Dynamic authorization control is achieved via wildcard support (`*:*` or `users:*`).
- **Temporary Roles:** Roles can be assigned to users for a specific, limited duration (`expires_at`).

### 2. Audit Engine (Audit Logging)

- All critical state changes within the system (CREATE, UPDATE, DELETE, LOGIN) are atomically recorded in the database.
- Logs capture the IP address, User-Agent, Session ID, pre/post-operation JSON states (`before_state`, `after_state`), and detailed error messages.
- Snapshots of deleted users are preserved in the logs for legal compliance and operational tracking.

### 3. Security & Auth Mechanism

- Implements an Access & Refresh JWT architecture.
- **Token Revocation:** Upon logout or token refresh, invalidated tokens are committed to the `token_blacklist` table.
- **Brute-Force Protection:** Accounts are automatically locked (`is_locked=True`) after 5 consecutive failed login attempts.

---

## Installation & Execution

### 1. Install Dependencies

It is highly recommended to run the system within an isolated virtual environment.

```bash
pip install fastapi uvicorn sqlalchemy pydantic python-jose passlib bcrypt python-multipart
```

### 2. Run the Project

```bash
python rbac_audit_system.py
```

### 3. Access API Documentation

```text
http://localhost:8000/docs
```

### Default Credentials

```text
Username: admin
Password: Admin@123!
```

---

# Advanced RBAC & Audit System

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green.svg)](https://fastapi.tiangolo.com)

**Repo:** https://github.com/Omimas/advanced-rbac-audit-system/

---

## Basit Özet

FastAPI kullanılarak geliştirilmiş, kurumsal düzeyde Hiyerarşik Rol Tabanlı Erişim Kontrolü (RBAC) ve kapsamlı denetim (audit) altyapısı sunan bir kimlik doğrulama ve yetkilendirme servisidir. Kullanıcı işlemleri atomik olarak loglanır, JWT tabanlı kimlik doğrulama token kara liste (blacklist) mekanizmasıyla güçlendirilmiştir.

---

## Teknik Açıklama: Mimari ve Yaklaşım

Sistem üç ana güvenlik motoru üzerine inşa edilmiştir:

### 1. RBAC Engine (Rol ve İzin Yönetimi)

- **Hiyerarşik Roller:** Roller birbirinden miras alabilir (`parent_id`). Örneğin, `admin` rolü otomatik olarak `manager` yetkilerini kapsar.
- **Granüler Yetkilendirme:** İzinler `resource:action` formatında tanımlanır. Wildcard (`*:*` veya `users:*`) desteği ile dinamik yetki kontrolü sağlanır.
- **Geçici Roller:** Kullanıcılara belirli bir süre için (`expires_at`) rol atanabilir.

### 2. Audit Engine (Denetim Kayıtları)

- Sistemdeki tüm kritik durum değişiklikleri (CREATE, UPDATE, DELETE, LOGIN) veritabanına atomik olarak kaydedilir.
- Loglar; IP adresi, User-Agent, Session ID, işlem öncesi/sonrası JSON durumları (`before_state`, `after_state`) ve hata detaylarını içerir.
- Hukuki ve operasyonel takip için silinmiş kullanıcı snapshot'ları korunur.

### 3. Güvenlik ve Kimlik Doğrulama Mekanizması

- Access & Refresh JWT mimarisi kullanılmıştır.
- **Token Revocation:** Çıkış yapıldığında veya token yenilendiğinde eski tokenlar `token_blacklist` tablosuna eklenir.
- **Brute-Force Koruması:** 5 ardışık hatalı giriş sonrasında hesap otomatik olarak kilitlenir (`is_locked=True`).

---

## Kurulum ve Çalıştırma

### 1. Bağımlılıkların Yüklenmesi

Sistemi sanal bir ortamda (virtual environment) çalıştırmanız önerilir.

```bash
pip install fastapi uvicorn sqlalchemy pydantic python-jose passlib bcrypt python-multipart
```

### 2. Projeyi Çalıştırın

```bash
python rbac_audit_system.py
```

### 3. API Dokümantasyonu

```text
http://localhost:8000/docs
```

### Varsayılan Admin Bilgileri

```text
Kullanıcı Adı: admin
Şifre: Admin@123!
```
