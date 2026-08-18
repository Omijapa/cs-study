## Transaction (트랜잭션)

데이터베이스에서 하나의 작업을 수행하기 위해 여러 SQL 명령을 하나의 작업 단위로 묶어 처리하는 것

하나의 Transaction에 포함된 작업은 모두 정상적으로 수행되거나, 문제가 발생하면 전체 작업을 취소하여 데이터의 일관성을 유지

### 특징 (ACID)

#### 1. 원자성 (Atomicity)

Transaction에 포함된 작업이 모두 수행되거나 하나도 수행되지 않도록 하는 성질

→ 일부 작업만 성공하고 나머지가 실패하면 이미 수행된 작업도 Rollback하여 이전 상태로 되돌림

#### 2. 일관성 (Consistency)

Transaction이 완료된 이후에도 데이터베이스가 정상적인 상태를 유지하도록 하는 성질

→ Transaction을 수행하기 전과 수행한 후에 데이터베이스의 규칙과 제약 조건이 유지되어야 함

#### 3. 독립성 (Isolation)

여러 Transaction이 동시에 실행되더라도 각 Transaction의 작업이 서로 영향을 주지 않도록 하는 성질

→ 한 Transaction이 처리 중인 데이터를 다른 Transaction이 임의로 사용하여 문제가 발생하지 않도록 함

#### 4. 지속성 (Durability)

Transaction이 성공적으로 완료되면 그 결과가 데이터베이스에 영구적으로 반영되는 성질

### Commit

Transaction의 작업을 정상적으로 완료하고 변경 내용을 데이터베이스에 확정하는 연산

→ Commit이 수행되면 해당 Transaction의 변경 내용을 되돌리기 어려움

### Rollback

Transaction에서 수행한 작업을 취소하고 이전의 일관된 상태로 되돌리는 연산

→ Transaction을 수행하는 도중 오류가 발생하거나 작업을 정상적으로 완료하지 못한 경우 사용함

## Transaction Isolation Level (트랜잭션 격리 단계)

여러 Transaction이 동시에 실행될 때 서로 얼마나 영향을 주고받을 수 있는지를 정하는 기준

Isolation Level이 높아질수록 다른 Transaction의 변경 내용을 덜 볼 수 있어 데이터의 일관성은 높아지지만 동시처리 성능은 낮아질 수 있음

### 문제 (후에 Level에서 나올 현상들)

- Dirty Read : 다른 Transaction이 Commit하지 않은 데이터를 읽는 현상
- Non-Repeatable Read : 같은 Transaction에서 같은 데이터를 다시 읽었을 때 값이 변경되어 있는 현상
- Phantom Read : 같은 조건으로 다시 조회했을 때 이전에는 없던 행이 추가되거나 사라지는 현상

(가장 낮은 Level)

### 1. Read Uncommitted

다른 Transaction이 아직 Commit하지 않은 데이터도 읽을 수 있는 수준

아직 확정되지 않은 데이터를 읽을 수 있기 때문에 Dirty Read가 발생할 수 있음

### 2. Read Committed

다른 Transaction이 Commit한 데이터만 읽을 수 있는 수준

Dirty Read는 방지할 수 있지만, 같은 Transaction에서 같은 데이터를 두 번 조회했을 때 다른 결과가 나오는 Non-Repeatable Read가 발생할 수 있음

### 3. Repeatable Read

하나의 Transaction에서 같은 데이터를 여러 번 조회하더라도 동일한 결과를 읽을 수 있도록 하는 수준

Read Committed보다 높은 격리 수준이며, Dirty Read와 Non-Repeatable Read를 방지함

### 4. Serializable

Transaction을 가장 엄격하게 분리하여 동시에 실행되는 Transaction이 서로 영향을 주지 않도록 하는 수준

데이터의 일관성을 가장 높게 유지할 수 있지만, Transaction을 순차적으로 처리하는 것과 비슷해져 동시 처리 성능이 가장 낮음

(가장 높은 Level)