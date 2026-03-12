Intelligent Emergency Room Scheduling System Using Hybrid Risk-Aware Algorithm

Algorithm Pseudocode
The following pseudocode describes the core logic of the Hybrid Risk-Aware Emergency Room Scheduling Algorithm implemented in this project.

Text 

Algorithm: Hybrid_ER_Scheduler

Input:
    patients.csv dataset
    list of doctors with specialization
Output:
    optimized treatment schedule (submission.json)

1. Read patient dataset from patients.csv
2. Sort patients based on arrival_time
3. Initialize:
       current_time ← 0
       doctor_free_time for each doctor ← 0
       TRAUMA_QUEUE ← empty
       CARDIO_QUEUE ← empty
       GLOBAL_QUEUE ← empty

4. While untreated patients exist:

5.     Add all patients where arrival_time ≤ current_time
       into their respective queues

6.     If no doctor is available:
           move current_time to next event
           (either next arrival or doctor availability)

7.     For each waiting patient:
           waiting_time ← current_time − arrival_time

8.     Compute priority score:
           priority = (3 × severity)
                    + (2 × waiting_time)
                    − (0.5 × treatment_time)

9.     Select Top-K highest priority patients

10.    For each candidate in Top-K:
           simulate assignments with lookahead depth (2–3)
           calculate estimated ER risk

11.    Select candidate with minimum predicted risk

12.    Assign selected patient to compatible doctor

13.    Update:
           doctor_free_time
           treatment start and end time

14.    Advance current_time to next scheduling event

15. End While

16. Compute Total_ER_Risk:
        Σ(severity × waiting_time)

17. Export final schedule to submission.json

Time Complexity Analysis
The computational complexity of the scheduling algorithm depends on several factors, including the number of patients and the lookahead depth.
Let:
N = number of patients
K = number of top candidates considered
D = lookahead depth

Sorting Phase
Patients are initially sorted by arrival time.
Complexity:
O(N log N)

Priority Calculation
For each scheduling cycle, priority scores are calculated for waiting patients.
Worst case:
O(N)
Top-K Selection
Selecting the top K candidates from the waiting queue.
Using heap or partial sort:
O(N log K)

Lookahead Simulation
The lookahead stage evaluates possible future assignments.
Worst-case complexity:
O(K^D)

However, since both K and D are small constants (K = 3, D ≤ 3), the simulation remains computationally manageable.

Overall Complexity
The approximate overall complexity is:
O(N log N + scheduling_cycles × (N log K + K^D))

Since scheduling cycles are bounded by number of patients, the algorithm remains efficient for typical emergency department workloads.

┌───────────────────────┐
                │     Input Dataset      │
                │      patients.csv      │
                └────────────┬───────────┘
                             │
                             ▼
                ┌───────────────────────┐
                │   Patient Intake       │
                │  Arrival Processing    │
                └────────────┬───────────┘
                             │
                             ▼
                ┌───────────────────────┐
                │   Queue Manager        │
                │                       │
                │  TRAUMA_QUEUE         │
                │  CARDIO_QUEUE         │
                │  GLOBAL_QUEUE         │
                └────────────┬───────────┘
                             │
                             ▼
                ┌───────────────────────┐
                │  Priority Calculator   │
                │  Risk Evaluation       │
                └────────────┬───────────┘
                             │
                             ▼
                ┌───────────────────────┐
                │ Hybrid Scheduling      │
                │ Engine                 │
                │                       │
                │ Greedy Priority       │
                │ + Lookahead Simulation│
                └────────────┬───────────┘
                             │
                             ▼
                ┌───────────────────────┐
                │ Doctor Resource       │
                │ Allocation Module     │
                └────────────┬───────────┘
                             │
                             ▼
                ┌───────────────────────┐
                │ Final Treatment       │
                │ Schedule Generator    │
                │ submission.json       │
                └───────────────────────┘

System Workflow Summary
The system processes incoming emergency patients through the following pipeline:
Patient intake reads real-time arrivals.
Patients are categorized into specialized queues.
Priority scores are dynamically computed.
Top candidate patients are selected.
Lookahead simulation predicts scheduling outcomes.
The system assigns the most optimal patient to the available doctor.
Doctor availability and system time are updated.
The process repeats until all patients are treated.
The final optimized schedule is exported.

Future Improvements
Although the current algorithm effectively reduces ER risk, several improvements could further enhance the system:
Reinforcement learning based scheduling
Real-time hospital sensor integration
Multi-hospital resource coordination
Adaptive severity estimation using AI models
These enhancements could enable the system to support fully automated intelligent hospital management systems in the future.
