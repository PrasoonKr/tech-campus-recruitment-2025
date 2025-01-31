# Discussions

## Solutions Considered

In this section, we explore the different approaches considered to solve the problem and their pros and cons.

### 1. **Approach 1: Single-threaded Log Processing**
- **Description**: The idea was to read the entire log file sequentially in a single thread and filter the lines based on the target date.
- **Pros**:
  - Simple and easy to implement.
  - Minimal setup required.
- **Cons**:
  - Very slow for large log files, as it reads and processes everything sequentially.
  - Not scalable to handle bigger files efficiently.
- **Why it was not chosen**: Although simple, this approach couldn't handle large log files efficiently, especially in a production setting where file sizes can be substantial. Performance was a major concern.

### 2. **Approach 2: Multi-threaded Log Processing with File Chunking**
- **Description**: This approach involved splitting the log file into chunks, processing each chunk in a separate thread, and filtering lines based on the target date in parallel.
- **Pros**:
  - Significantly faster due to parallel processing.
  - Scalable to larger files, as multiple threads work on different portions of the file.
  - Better CPU utilization.
- **Cons**:
  - Requires more complex code for managing multiple threads.
  - Potential race conditions or file corruption if not handled properly.
- **Why it was not chosen**: While this approach provided the best performance, the complexity of managing threads and ensuring that file writes were done safely was a significant concern. 

### 3. **Approach 3: Optimized Multi-threaded Log Processing with Worker Threads**
- **Description**: This approach combined multi-threaded processing with worker threads to divide the log file into manageable chunks. Each worker thread processes its chunk independently, and results are written to a final output file.
- **Pros**:
  - High performance, processing log files in parallel without blocking the main thread.
  - Cleaner and more maintainable code with worker threads.
  - Automatic load balancing through the number of available CPU cores.
- **Cons**:
  - Still more complex compared to a single-threaded solution.
  - Some overhead for managing worker threads and their communication.
- **Why it was chosen**: This solution offered the best balance of performance and maintainability. It utilized worker threads to take advantage of multi-core processors while maintaining code clarity. Additionally, it scaled well with file sizes and system resources.

## Final Solution Summary

### **Chosen Solution**: Optimized Multi-threaded Log Processing with Worker Threads

- **Chosen Solution**: The final solution involves using worker threads to process chunks of the log file in parallel. Each worker thread filters the log lines based on the target date and appends the results to the final output file.
- **Why it was chosen**:
  - It handled large log files efficiently by processing them in parallel.
  - It ensured good utilization of system resources (CPU cores) through dynamic worker thread allocation.
  - The code structure is clean and maintainable, with clear separation between the main thread and worker threads.
  - This solution was also flexible enough to handle different file sizes and adjust dynamically based on system resources.
  
## Steps to Run

Follow these steps to run the solution on your machine.

### Prerequisites
Before running the solution, ensure you have the following installed:
- [Node.js](https://nodejs.org/) (version 14 or higher)

### Step 1: Clone the Repository
Clone the repository to your local machine:
```bash
git clone https://github.com/PrasoonKr/tech-campus-recruitment-2025.git
```

### Step 2: Install Dependencies
Navigate to the project directory and install the dependencies:
```bash
cd tech-campus-recruitment-2025/src
```

### Step 3: Run the Solution
To run the solution, use the following command:
```bash
node extract_logs.js <target_date>
```

### Example
```bash
node extract_logs.js 2024-01-01
```

### Output
The solution will create an output file with the name `output_2024-01-01.txt` in the `output` directory.
