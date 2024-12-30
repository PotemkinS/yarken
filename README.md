# Обзор контракта RentalDeposit

Контракт `RentalDeposit` позволяет арендодателям и арендаторам управлять арендными депозитами на блокчейне Ethereum. Контракт предоставляет функции для создания договора аренды, одобрения возврата депозита, снятия депозита и возврата депозита арендатором.

## Основные концепции

- **Договор аренды (Lease)**: Договор между арендодателем и арендатором, с указанием суммы депозита, дат начала и окончания аренды, а также процесса одобрения возврата депозита.
- **Управление депозитом**: Арендодатель может снять депозит после окончания аренды, а арендатор может вернуть депозит арендодателю после одобрения.
- **События**: Контракт генерирует события для создания договора аренды, снятия депозита и возврата депозита для обеспечения прозрачности.

## Функции контракта

### 1. `createLease(address _landlord, uint256 _depositAmount, uint256 _startDate, uint256 _endDate)`
- **Описание**: Эта функция позволяет арендатору создать договор аренды, указав адрес арендодателя, сумму депозита, дату начала и окончания аренды. Арендатор должен отправить точную сумму депозита в эфире.
- **Требования**:
  - Отправленная сумма должна соответствовать депозиту.
  - Дата начала должна быть раньше даты окончания.
- **События**:
  - `LeaseCreated`: Генерируется, когда договор аренды успешно создан.

### 2. `approveDepositReturn(uint256 _leaseId)`
- **Описание**: Эта функция позволяет арендодателю одобрить возврат депозита арендатору. Арендодатель может одобрить возврат только тогда, когда договор аренды активен.
- **Требования**:
  - Вызвавший функцию должен быть арендодателем.
  - Договор аренды должен быть активен.
- **События**:
  - `LandlordApproved`: Генерируется, когда арендодатель одобряет возврат депозита.

### 3. `withdrawDeposit(uint256 _leaseId)`
- **Описание**: Эта функция позволяет арендодателю снять депозит после окончания срока аренды. Депозит отправляется на адрес арендодателя.
- **Требования**:
  - Вызвавший функцию должен быть арендодателем.
  - Договор аренды должен быть завершен.
  - Депозит еще не был снят.
- **События**:
  - `DepositWithdrawn`: Генерируется, когда арендодатель снимает депозит.

### 4. `returnDeposit(uint256 _leaseId)`
- **Описание**: Эта функция позволяет арендатору вернуть депозит арендодателю после окончания аренды, но только после того, как арендодатель одобрит возврат.
- **Требования**:
  - Вызвавший функцию должен быть арендатором.
  - Договор аренды должен быть завершен, арендодатель должен одобрить возврат.
  - Депозит еще не был снят.
- **События**:
  - `DepositReturned`: Генерируется, когда депозит возвращается арендатору.

### 5. `getLeaseDetails(uint256 _leaseId)`
- **Описание**: Эта функция предоставляет подробную информацию о конкретном договоре аренды по его ID.
- **Требования**:
  - Договор аренды с указанным ID должен существовать.
- **Возвращаемые данные**: Функция возвращает информацию о договоре аренды, включая адреса арендодателя и арендатора, сумму депозита, даты начала и окончания аренды, статус аренды, одобрение арендодателя и статус снятия депозита.

### 6. `getLeaseCount()`
- **Описание**: Эта функция возвращает общее количество договоров аренды, созданных в контракте.
- **Возвращаемые данные**: Количество договоров аренды.

## Модификаторы

### 1. `onlyLandlord(uint256 _leaseId)`
- Обеспечивает, чтобы только арендодатель мог выполнять определенные действия (например, одобрить возврат депозита или снять депозит).

### 2. `onlyTenant(uint256 _leaseId)`
- Обеспечивает, чтобы только арендатор мог вернуть депозит.

### 3. `leaseExists(uint256 _leaseId)`
- Проверяет, существует ли договор аренды с указанным ID.

## События

- `LeaseCreated`: Генерируется при создании нового договора аренды с информацией о арендодателе, арендаторе, сумме депозита и датах аренды.
- `DepositWithdrawn`: Генерируется, когда арендодатель снимает депозит.
- `DepositReturned`: Генерируется, когда арендатор возвращает депозит.
- `LandlordApproved`: Генерируется, когда арендодатель одобряет возврат депозита.

## Обработка ошибок

- Контракт использует инструкции `require`, чтобы убедиться, что только соответствующая сторона (арендодатель или арендатор) может выполнять определенные действия.
- Он также проверяет, что договор аренды активен, что даты правильные и что депозит может быть снят или возвращен только в нужный момент.

## Пример использования

1. **Создание договора аренды**: Арендатор может создать договор аренды, вызвав функцию `createLease` и отправив сумму депозита.
   
2. **Одобрение возврата депозита**: После окончания аренды арендодатель может одобрить возврат депозита, вызвав функцию `approveDepositReturn`.
   
3. **Снятие депозита**: После окончания аренды арендодатель может вызвать функцию `withdrawDeposit` для получения депозита.

4. **Возврат депозита**: После одобрения арендодателем арендатор может вызвать функцию `returnDeposit` для возврата депозита.

---

Этот контракт представляет собой простую и прозрачную систему для управления арендными депозитами на блокчейне, обеспечивая обеим сторонам — арендодателю и арендатору — четкий процесс для обработки депозитов.