# AMR ⇄ PLC ⇄ 電動門 握手流程圖

```mermaid
sequenceDiagram
    participant AMR as AMR 無人搬運車
    participant PLC as PLC 控制器
    participant Door as 電動門

    Note over AMR,Door: t0 抵達門區安全點

    AMR->>PLC: OpenReq(jobId, doorId)
    PLC-->>AMR: Ack(reqId, eta≈2s)
    PLC->>Door: DoorOpen()
    Door-->>PLC: DoorStatus=OPEN
    PLC-->>AMR: PermitPass(timeout=5s)
    AMR->>Door: PassThrough()
    AMR-->>PLC: PassComplete()
    PLC->>Door: DoorClose()
    Door-->>PLC: DoorStatus=CLOSED
    PLC-->>AMR: SessionClose()
