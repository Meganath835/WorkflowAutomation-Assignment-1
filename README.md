# BPMN Process Models

A collection of Business Process Model and Notation (BPMN) models created using **Camunda Modeler**. The models demonstrate how business processes can be represented using start events, tasks, exclusive gateways, sequence flows, and end events.

## Scenarios

| # | Process                          | Key Decisions                            |
| - | -------------------------------- | ---------------------------------------- |
| 1 | Employee Leave Approval          | Leave balance and manager approval       |
| 2 | Online Purchase Order Processing | Product availability and payment status  |
| 3 | IT Service Request               | Problem severity and internal resolution |

---

# Scenario 1:Employee Leave Approval

## Overview

The Employee Leave Approval process models how an employee's leave request is handled by an organization's HR system.

The process begins when an employee submits a leave request. The HR system first checks whether the employee has sufficient leave balance. If sufficient leave is available, the request is forwarded to the manager for approval. The manager's decision determines whether the leave is approved or rejected.

If there is insufficient leave balance, the request does not proceed to managerial approval and the employee is immediately notified.

## Process Flow

The process follows these stages:

1. **Leave Request Submitted** — The employee initiates the process by submitting a leave request.
2. **Check Leave Balance** — The HR system checks the employee's available leave balance.
3. **Leave Balance Sufficient?** — An exclusive gateway determines whether the employee has enough leave available.
4. **Send Leave Request to Manager** — If sufficient balance exists, the request is forwarded to the manager.
5. **Manager Approved?** — The manager reviews the request and makes the approval decision.
6. **Update Employee Leave Balance** — If approved, the system updates the employee's remaining leave balance.
7. **Send Notification** — The employee receives an approval, rejection, or insufficient-balance notification.
8. **Process Ends** — The appropriate end event is reached.

## Decision-Making

### Decision 1: Is the Leave Balance Sufficient?

| Condition | Process Path                                                   |
| --------- | -------------------------------------------------------------- |
| **Yes**   | Send the leave request to the manager for approval.            |
| **No**    | Send an insufficient-balance notification and end the process. |

The first Exclusive Gateway prevents requests from proceeding to managerial approval when the employee does not have enough leave available.

### Decision 2 — Did the Manager Approve?

| Condition    | Process Path                                                |
| ------------ | ----------------------------------------------------------- |
| **Approved** | Update the leave balance → Send approval notification → End |
| **Rejected** | Send rejection notification → End                           |

If the manager approves the request, the employee's leave balance is updated before the approval notification is sent. If the manager rejects the request, the leave balance remains unchanged and the employee is notified of the rejection.

## Process Paths

### Path 1:Insufficient Leave Balance

```text
Leave Request Submitted
        ↓
Check Leave Balance
        ↓
Leave Balance Sufficient?
        ↓ No
Insufficient Balance Notification
        ↓
END
```

### Path 2: Leave Approved

```text
Leave Request Submitted
        ↓
Check Leave Balance
        ↓
Leave Balance Sufficient?
        ↓ Yes
Send Leave Request to Manager
        ↓
Manager Approved?
        ↓ Approved
Update Employee Leave Balance
        ↓
Approval Notification
        ↓
END
```

### Path 3 :Leave Rejected

```text
Leave Request Submitted
        ↓
Check Leave Balance
        ↓
Leave Balance Sufficient?
        ↓ Yes
Send Leave Request to Manager
        ↓
Manager Approved?
        ↓ Rejected
Rejection Notification
        ↓
END
```

## BPMN Diagram

![Employee Leave Approval](Scenario-1/employee-leave-approval.png)

**BPMN Model:** [`employee-leave-approval.bpmn`](Scenario-1/employee-leave-approval.bpmn)

---

# Scenario 2 :Online Purchase Order Processing

## Overview

The Online Purchase Order Processing process models the workflow of an online purchase from the moment a customer places an order until the product is shipped.

The system first checks whether the requested product is available. If the product is unavailable, the customer is notified and the process ends.

If the product is available, payment is processed. A second decision determines whether the payment was successful. Successful payment allows the order to proceed through confirmation, product preparation, shipping, and shipping notification. A failed payment terminates the process after notifying the customer.

## Process Flow

The process follows these stages:

1. **Customer Places Order** — The customer submits an online order.
2. **Check Product Availability** — The system checks whether the requested product is in stock.
3. **Product Available?** — An exclusive gateway determines whether the order can proceed.
4. **Process Payment** — If the product is available, the system attempts to process the customer's payment.
5. **Payment Successful?** — An exclusive gateway determines whether payment was completed successfully.
6. **Confirm Order** — A successful payment results in confirmation of the order.
7. **Prepare Product for Shipment** — The product is prepared and packaged for dispatch.
8. **Ship Order** — The prepared order is shipped to the customer.
9. **Send Shipping Confirmation** — The customer is notified that the order has been shipped.
10. **Process Ends** — The appropriate end event is reached.

## Decision-Making

### Decision 1: Is the Product Available?

| Condition | Process Path                                           |
| --------- | ------------------------------------------------------ |
| **Yes**   | Continue to payment processing.                        |
| **No**    | Send an out-of-stock notification and end the process. |

This decision prevents the system from processing an order for a product that cannot currently be supplied.

### Decision 2: Was Payment Successful?

| Condition | Process Path                                                                    |
| --------- | ------------------------------------------------------------------------------- |
| **Yes**   | Confirm order → Prepare product → Ship order → Send shipping confirmation → End |
| **No**    | Send payment failure notification → End                                         |

Payment must be successful before the order can proceed to confirmation and shipment.

## Process Paths

### Path 1: Product Unavailable

```text
Customer Places Order
        ↓
Check Product Availability
        ↓
Product Available?
        ↓ No
Out-of-Stock Notification
        ↓
END
```

### Path 2: Payment Failure

```text
Customer Places Order
        ↓
Check Product Availability
        ↓
Product Available?
        ↓ Yes
Process Payment
        ↓
Payment Successful?
        ↓ No
Payment Failure Notification
        ↓
END
```

### Path 3 :Successful Order

```text
Customer Places Order
        ↓
Check Product Availability
        ↓
Product Available?
        ↓ Yes
Process Payment
        ↓
Payment Successful?
        ↓ Yes
Confirm Order
        ↓
Prepare Product for Shipment
        ↓
Ship Order
        ↓
Shipping Confirmation
        ↓
END
```

## BPMN Diagram

![Online Purchase Order Processing](Scenario-2/online-purchase-order.png)

**BPMN Model:** [`online-purchase-order.bpmn`](Scenario-2/online-purchase-order.bpmn)

---

# Scenario 3:IT Service Request

## Overview

The IT Service Request process models how an organization's IT help desk handles an employee's technical support request.

The process begins when an employee reports an IT problem. The help desk registers the request and evaluates the severity of the issue.

The severity determines which type of technician receives the request. Low-severity issues are assigned to a support technician, while high-severity issues are assigned to a senior technician.

After the appropriate technician investigates the problem, a second decision determines whether the issue can be resolved internally. If it can, the technician fixes the problem. If it cannot, the issue is escalated to an external service provider.

Both resolution paths eventually merge back into the same process, where the help desk updates the request status and notifies the employee.

## Process Flow

The process follows these stages:

1. **Employee Reports IT Problem** — The employee initiates the support process.
2. **Submit IT Support Request** — The employee formally submits the support request.
3. **Register IT Request** — The help desk records the request in the IT support system.
4. **Check Problem Severity** — The help desk evaluates the severity of the reported issue.
5. **Problem Severity?** — An exclusive gateway determines which type of technician should handle the request.
6. **Assign to Support Technician** — Low-severity issues are assigned to a regular support technician.
7. **Assign to Senior Technician** — High-severity issues are assigned to a senior technician.
8. **Investigate Problem** — The assigned technician investigates the issue.
9. **Problem Resolved Internally?** — An exclusive gateway determines whether the organization can resolve the issue internally.
10. **Fix Problem** — Internally resolvable issues are fixed by the technician.
11. **Escalate to External Service Provider** — Issues that cannot be resolved internally are transferred to an external provider.
12. **Update Request Status** — The help desk updates the status of the support request.
13. **Send Resolution Notification** — The employee is informed about the resolution or handling of the issue.
14. **Process Ends** — The process reaches the final end event.

## Decision-Making

### Decision 1: What Is the Problem Severity?

| Condition         | Process Path                    |
| ----------------- | ------------------------------- |
| **Low Severity**  | Assign to a support technician. |
| **High Severity** | Assign to a senior technician.  |

The purpose of this decision is to ensure that the problem is assigned to an appropriate level of technical expertise.

Both paths eventually merge into **Investigate Problem**, because regardless of the technician assigned, the next step is to investigate the reported issue.

### Decision 2 :Can the Problem Be Resolved Internally?

| Condition | Process Path                                          |
| --------- | ----------------------------------------------------- |
| **Yes**   | Fix the problem internally.                           |
| **No**    | Escalate the problem to an external service provider. |

Both paths then merge into **Update Request Status** because the help desk must update the support record regardless of whether the issue was resolved internally or externally.

## Process Paths

### Path 1 :Low Severity + Internal Resolution

```text
Employee Reports IT Problem
        ↓
Submit IT Support Request
        ↓
Register IT Request
        ↓
Check Problem Severity
        ↓
Problem Severity?
        ↓ Low
Assign to Support Technician
        ↓
Investigate Problem
        ↓
Problem Resolved Internally?
        ↓ Yes
Fix Problem
        ↓
Update Request Status
        ↓
Resolution Notification
        ↓
END
```

### Path 2 :High Severity + Internal Resolution

```text
Employee Reports IT Problem
        ↓
Submit IT Support Request
        ↓
Register IT Request
        ↓
Check Problem Severity
        ↓
Problem Severity?
        ↓ High
Assign to Senior Technician
        ↓
Investigate Problem
        ↓
Problem Resolved Internally?
        ↓ Yes
Fix Problem
        ↓
Update Request Status
        ↓
Resolution Notification
        ↓
END
```

### Path 3: Low Severity + External Escalation

```text
Employee Reports IT Problem
        ↓
Submit IT Support Request
        ↓
Register IT Request
        ↓
Check Problem Severity
        ↓
Problem Severity?
        ↓ Low
Assign to Support Technician
        ↓
Investigate Problem
        ↓
Problem Resolved Internally?
        ↓ No
Escalate to External Service Provider
        ↓
Update Request Status
        ↓
Resolution Notification
        ↓
END
```

### Path 4: High Severity + External Escalation

```text
Employee Reports IT Problem
        ↓
Submit IT Support Request
        ↓
Register IT Request
        ↓
Check Problem Severity
        ↓
Problem Severity?
        ↓ High
Assign to Senior Technician
        ↓
Investigate Problem
        ↓
Problem Resolved Internally?
        ↓ No
Escalate to External Service Provider
        ↓
Update Request Status
        ↓
Resolution Notification
        ↓
END
```

## BPMN Diagram

![IT Service Request](Scenario-3/it-service-request.png)

**BPMN Model:** [`it-service-request.bpmn`](Scenario-3/it-service-request.bpmn)

---

# BPMN Elements Used

The three scenarios demonstrate the following BPMN elements:

| BPMN Element          | Purpose                                                                                             |
| --------------------- | --------------------------------------------------------------------------------------------------- |
| **Start Event**       | Indicates where the business process begins.                                                        |
| **Task**              | Represents an activity performed by an employee, system, manager, technician, or other participant. |
| **Exclusive Gateway** | Represents a decision where exactly one available path is selected based on a condition.            |
| **Sequence Flow**     | Connects BPMN elements and represents the order in which activities occur.                          |
| **End Event**         | Indicates that a particular process path has finished.                                              |

## Exclusive Gateway Logic

The Exclusive Gateways in these models represent **mutually exclusive business decisions**.

For example:

```text
                 ┌──────────────┐
                 │   Gateway    │
                 └──────┬───────┘
                        │
                 ┌──────┴──────┐
                 ↓             ↓
                YES            NO
                 │             │
              Path A         Path B
```

Only one path is followed based on the outcome of the decision.

This is used throughout the models to represent decisions such as:

* Is there sufficient leave balance?
* Did the manager approve the leave?
* Is the product available?
* Was payment successful?
* What is the IT problem severity?
* Can the IT problem be resolved internally?

## Conclusion

These BPMN models demonstrate how real-world business processes can be represented using structured process flows and decision points.

Each model begins with a **Start Event**, progresses through a sequence of **Tasks**, uses **Exclusive Gateways** to represent business decisions, and terminates through one or more **End Events**.

The models also demonstrate different types of process behaviour:

* **Scenario 1** demonstrates approval and rejection decisions.
* **Scenario 2** demonstrates validation and transaction failure handling.
* **Scenario 3** demonstrates alternative assignment paths, internal resolution, external escalation, and path merging.

The corresponding `.bpmn` files can be opened and further edited using **Camunda Modeler**, while the diagram images provide a visual representation of each completed process.
