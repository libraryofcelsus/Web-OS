## A Secure, Self-Hosted Web Operating System 🔒

This project is a browser-based operating environment designed with a zero-trust security model. It simulates a traditional desktop, allowing users to run sandboxed web applications and manage files on a remote server where all data is end-to-end encrypted in transmission by default, with an optional encryption at rest that can be enabled and disabled quickly withing the OS Settings.

The architecture is centered around a frontend "microkernel" written in **React**, which manages services like a process scheduler, an IPC bus, and a virtual filesystem (VFS). The backend is a **Python/Flask** application that handles authentication, the E2EE handshake, and storage for the VFS.

### Core Technical Features

* **End-to-End Encryption (E2EE):** 
    * **Key Exchange:** A session is initiated with an **Elliptic Curve Diffie-Hellman (ECDH)** key exchange using the P-384 curve to establish a secure shared secret.
    * **Key Derivation:** A unique, ephemeral symmetric key is derived for the session using **HKDF-SHA256** (HMAC-based Key Derivation Function).
    * **Data Encryption:** All subsequent payloads for REST API calls, WebSocket messages, and file transfers are encrypted using **AES-256-GCM**, which provides both confidentiality and authenticity.

* **Application Sandboxing:** Applications are dynamically loaded and run in isolation. They interact with the kernel through a `syscall` interface, with permissions (e.g., VFS path access) defined declaratively in a manifest file.

* **Virtual File System (VFS):** The backend provides a sandboxed filesystem for each user. It also offers an optional, user-managed layer of **encryption-at-rest**, deriving a key from a master password using **PBKDF2** (600,000 iterations) to encrypt files before they are written to disk.

* **Authentication:** User authentication is handled via **Auth0**, with backend authorization enforced by validating **RS256-signed JWTs**. User roles and permissions are passed as trusted custom claims within the JWT.
