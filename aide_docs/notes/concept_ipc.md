# Tauri IPC System

## Core Concepts
- Async Message Passing
- JSON serialized req/resp
- Security-focused isolation
- Similar to web client-server

## IPC Primitives

1. Events
   - Bidirectional (FE ↔ Core)
   - Fire-and-forget msgs
   - Use: lifecycle/state changes
   - One-way communication

2. Commands
   - FFI-like abstraction
   - Frontend → Core only
   - JSON-RPC protocol
   - Similar to fetch API
   - Serializable args/returns

## Flow
- FE → Core: IPC Request
- Core → Handler: Command invoke
- Handler → Core: Serialized return
- Core → FE: Response

## Security
- Recipient controls execution
- Can reject malicious reqs
- No shared memory
- No direct func access