# ODA
<img width="752" height="752" alt="ODA_logo" src="https://github.com/user-attachments/assets/db973786-d3d7-4c33-8897-f58ae6c89cfd" />

# ODA - Open Decentralized Access

ODA es un **sistema de login universal descentralizado** que permite a los usuarios acceder a cualquier servicio web sin depender de intermediarios centralizados (Google, Facebook, Apple). El usuario es el **único propietario de su identidad digital** y de sus datos de acceso.

---

## Arquitectura Técnica de ODA

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

### Acceso a un Sitio Web (Cada vez)

1. Usuario visita un sitio con ODA y hace clic en "Acceder con ODA"
2. El servidor genera un **reto (challenge)** y lo envía a la wallet
3. La wallet **firma** el reto con la clave privada del usuario
4. El servidor **resuelve el DID** del usuario y verifica la firma
5. Si es válido, el servidor concede acceso y genera un token de sesión

## El Registro `did.jsonl` (Identidad)

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

### Gestión de Múltiples Dispositivos

1. El dispositivo principal genera un token de autorización
2. El nuevo dispositivo genera su propio par de claves
3. El dispositivo principal aprueba la vinculación
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
