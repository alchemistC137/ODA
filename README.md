# ODA
<img width="752" height="752" alt="ODA_logo" src="https://github.com/user-attachments/assets/db973786-d3d7-4c33-8897-f58ae6c89cfd" />

# ODA - Open Decentralized Access

## Resumen Ejecutivo del Proyecto

ODA es un **sistema de login universal descentralizado** que permite a los usuarios acceder a cualquier servicio web sin depender de intermediarios centralizados (Google, Facebook, Apple). El usuario es el **único propietario de su identidad digital** y de sus datos de acceso.

---

## 🎯 Objetivo del Proyecto

Desarrollar un **sistema de autenticación descentralizado** que:

1. Permita un login universal para todos los servicios
2. Elimine la dependencia de proveedores centralizados
3. Devuelva el control de la identidad al usuario
4. Sea de código abierto y auditable
5. Proporcione mecanismos robustos de auditoría y seguridad

---

## 🏗️ Arquitectura Técnica de ODA

### Componentes Principales

| Componente | Descripción | Tecnología |
|------------|-------------|------------|
| **Wallet ODA (PWA)** | Aplicación que gestiona las claves privadas y credenciales del usuario | PWA (React/TypeScript) |
| **Backend ODA** | Servidor que valida identidades y gestiona sesiones | Node.js/Python |
| **Servidor DID** | Aloja el registro verificable de identidades (`did.jsonl`) | Servidor web estándar |
| **Blockchain (Testnet)** | Ancla de confianza inmutable para DIDs | Ethereum Sepolia |
| **Audit Engine** | Sistema de logs descentralizado y verificable | Integrado en Wallet |
| **Web de Ejemplo (Demo)** | Sitio web que implementa ODA para demostración | React/Next.js |

---

## 🔐 El Flujo de Autenticación

### Registro de Identidad (Única vez)

1. El usuario genera un par de claves en su wallet
2. La wallet crea un **DID (Identificador Descentralizado)**
3. El DID se registra en el servidor `did:webvh`
4. El hash del registro se ancla en la blockchain (opcional)

### Acceso a un Sitio Web (Cada vez)

1. Usuario visita un sitio con ODA y hace clic en "Acceder con ODA"
2. El servidor genera un **reto (challenge)** y lo envía a la wallet
3. La wallet **firma** el reto con la clave privada del usuario
4. El servidor **resuelve el DID** del usuario y verifica la firma
5. Si es válido, el servidor concede acceso y genera un token de sesión

### Verificación de Credenciales (Acceso restringido)

1. El sitio web puede solicitar una **Credencial Verificable** (ej: "mayor de edad")
2. La wallet presenta la credencial usando **Pruebas de Conocimiento Cero (ZKP)**
3. El servidor verifica la credencial sin acceder a datos personales
4. El usuario obtiene acceso al contenido restringido

---

## 🗂️ El Registro `did.jsonl` (Identidad)

El archivo `did.jsonl` es el **corazón técnico** de la identidad del usuario.

### Contenido del `did.jsonl`

Cada entrada (entry) en el `did.jsonl` contiene:

- **`versionId`**: Identificador único de la versión del DID Document
- **`versionTime`**: Timestamp de la actualización
- **`didDoc`**: El documento DID actual
- **`updateKeys`**: Claves autorizadas para futuras actualizaciones
- **`nextKeyHashes`**: Hashes de claves de pre-rotación (seguridad avanzada)
- **`witnessSignatures`**: Firmas de los testigos (si se usan)
- **`hash`**: Hash de la entrada anterior (encadenamiento)

### Propósito del `did.jsonl`

| Función | Explicación |
|---------|-------------|
| **Integridad** | El encadenamiento de hashes permite detectar manipulaciones |
| **Verificabilidad** | Las firmas criptográficas demuestran autoría |
| **Historial** | Registro completo de cambios de identidad |
| **Auditoría** | Base para auditorías forenses de identidad |
| **Rotación de claves** | Permite cambiar claves sin cambiar el DID |

---

## 🔒 Seguridad de ODA

### Protección de la Clave Privada

| Capa | Medida | Tecnología |
|------|--------|------------|
| **Prevención** | Cifrado de la wallet | AES-256 + PBKDF2 |
| **Prevención** | Aislamiento por hardware | Secure Enclave / StrongBox |
| **Prevención** | Autenticación biométrica | Huella / Rostro |
| **Respuesta** | Rotación de claves | Actualización sin cambiar DID |
| **Respuesta** | Recuperación por guardianes | Shamir Secret Sharing |
| **Respuesta** | Copias de seguridad cifradas | Almacenamiento en cloud |

### Gestión de Múltiples Dispositivos

1. El dispositivo principal genera un token de autorización
2. El nuevo dispositivo genera su propio par de claves
3. El principal aprueba la vinculación
4. El DID Document se actualiza con la nueva clave pública
5. **Revocación**: Se puede revocar un dispositivo perdido desde otro

### Recuperación de Identidad

| Método | Descripción | Complejidad |
|--------|-------------|-------------|
| **Copia de seguridad cifrada** | Archivo cifrado en iCloud/Google Drive | Baja |
| **Código de recuperación** | Frase semilla (12 palabras) | Baja |
| **Múltiples dispositivos** | Vinculación de dispositivos autorizados | Media |
| **Shamir Secret Sharing** | Fragmentos de clave entre guardianes | Alta |

---

## 📊 Auditoría y Logs

### Logs de Acceso (En el Dispositivo del Usuario)

Los logs se almacenan **cifrados y firmados** en el dispositivo del usuario:

```json
{
  "did": "did:webvh:example.com:user123",
  "servicio": "https://ejemplo.com/recurso",
  "timestamp": "2026-06-29T10:30:00Z",
  "accion": "login",
  "resultado": "exitoso",
  "hash_anterior": "abc123...",
  "firma_servidor": "0x456def...",
  "firma_usuario": "0x789ghi..."
}
