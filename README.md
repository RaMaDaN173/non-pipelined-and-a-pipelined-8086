# non-pipelined-and-a-pipelined-8086
compare the execution times of a non-pipelined and a pipelined 8086 processor for a specified number of instructions.

Pipelining and non-pipelining are contrasting approaches to organizing and executing tasks, especially in the context of computer architecture and processing systems.

### Pipelined:
In a pipelined architecture, tasks or instructions are divided into a sequence of stages, each handling a specific operation. These stages are arranged in a pipeline, and multiple instructions can be processed simultaneously. As one stage completes its task, the output is passed to the next stage, allowing for continuous execution of instructions without waiting for the entire process of one instruction to finish before starting the next. Pipelining enhances throughput and overall system efficiency by parallelizing the execution of different instructions.

### Non-pipelined:
In contrast, a non-pipelined architecture processes tasks or instructions sequentially, one at a time, without overlapping operations. Each instruction must complete its entire execution before the next one begins. While this straightforward approach can be conceptually simpler, it may result in underutilized resources and longer overall processing times. Non-pipelined architectures are less able to exploit parallelism and may experience more significant idle periods in the execution of tasks.

In summary, pipelining introduces parallelism by breaking down tasks into stages and processing multiple instructions simultaneously, while non-pipelined systems execute tasks sequentially without overlapping operations. The choice between these architectures depends on the specific requirements of a given system and the balance between simplicity and performance optimization.

## This Java program
is designed to simulate and compare the execution times of a non-pipelined and a pipelined 8086 processor for a specified number of instructions. Here's a breakdown of the code:

1. User Input:

   * The program starts by prompting the user to enter the number of instructions to be simulated for both pipelined and non-pipelined scenarios.

2. Non-Pipelined Simulation:

   * The user is then prompted to enter the clock cycles required for each of the four stages (IF, ID, EX, WB) for each instruction.
   * The input is validated to ensure positive integers are entered.
   * The total number of clock cycles required for non-pipelined execution (TNP) is calculated.

3. Pipelined Simulation:

   * The program then simulates pipelined execution by applying the pipeline stages (IF, ID, EX, WB) to each instruction.
   * The execution time of the pipelined scenario is determined, and the result is stored in the variable TP.

4. Results Display:

   * The program then displays the clock cycle table for both the non-pipelined and pipelined scenarios.
   * It also prints out the total execution times for both scenarios (TNP for non-pipelined and TP for pipelined).
   * Finally, it calculates and displays the speedup achieved by pipelining.

5. Utility Methods:

   * Apply_pipeline: A method for updating the pipeline array based on the given instruction, clock cycles, and operation.
   * MAX_INDEX and MAX_INDEX_AtValue: Methods to find the index of the maximum value and the index of a specified value in an array, respectively.
   * Print: A utility method to print a 2D array.

6. Exception Handling:

   * The program includes exception handling to deal with invalid inputs such as non-integer values.
Note: The code assumes that the user provides valid input and does not perform extensive error checking beyond basic validation. Additionally, the simulation is a simplified representation and may not capture all aspects of real-world processor behavior.

# CODE
    import java.util.Scanner;

    public class Pipelined_nonpipelined {
    public static void main(String[] args) {
        boolean Test = false;
        int NumberOf_instruction = 0;
        System.out.println("Project To pipelined 8086 & nonpipelined 8086");
        do {/*if test became true try again*/
            try {
                Scanner sc1 = new Scanner(System.in);
                System.out.print("Enter Number of Instructions  :  ");
                NumberOf_instruction = sc1.nextInt();

                if (NumberOf_instruction <= 0) {/* Enter value negative */
                    System.out.println("Please Enter Value Greater than 0\n");
                    Test = true;
                } else {
                    Test = false;
                }
            } catch (Exception e1) {/* enter any value not integer */
                System.out.println("Enter  integer Value");
                Test = true;
            }
        } while (Test);
        int Table/*array of clock circle for each instruction level*/[][] = new int[NumberOf_instruction][4/*number of instruction level[IF,ID,EX,WB]*/];
        int TNP/*Variable of number of circle that take in nonpipelined*/ = 0;
        Test = false;
        do {
            TNP = 0;/* start from zero again */
            int Value_Instruction = 0;
            try {
                for (int Row = 0; Row < NumberOf_instruction; Row++) {
                    for (int column = 0; column < 4; column++) {
                        if (column == 0) {
                            System.out.print("fetch \"IF\" of ");
                        } else if (column == 1) {
                            System.out.print("decode \"ID\" of ");
                        } else if (column == 2) {
                            System.out.print("Execution \"EX\" of ");
                        } else {
                            System.out.print("write back (Store) \"WB\" of ");
                        }
                        System.out.print("Instruction number " + (Row + 1) + " :  ");

                        Scanner sc2 = new Scanner(System.in);
                        Value_Instruction = sc2.nextInt();
                        if (Value_Instruction <= 0) {
                            System.out.println("Enter Value Greater than 0");
                            Test = true;
                            break;
                        } else {
                            TNP += Value_Instruction;
                            Table[Row][column] = Value_Instruction;
                            System.out.println("");
                            Test = false;
                        }
                    }
                    if (Value_Instruction <= 0) {
                        Test = true;
                        System.out.println("#######Fill The Table again#######\n");
                        break;
                    }
                }
            } catch (Exception e1) {
                System.out.println("Enter  integer Value");
                System.out.println("#######Fill The Table again#######\n");
                Test = true;
            }

        } while (Test);
        int Table_Pipelined[][] = new int[4][TNP];
        for (int operation = 1; operation <= 4; operation++) {
            for (int instruction = 0; instruction < Table.length; instruction++) { // two loop at once change cell in array
                Table_Pipelined = Apply_pipeline(Table_Pipelined, instruction + 1, Table[instruction][operation - 1], operation);
            }
        }
        int TP = MAX_INDEX_AtValue(Table_Pipelined[NumberOf_instruction - 1], NumberOf_instruction) + 1;
        System.out.println("[IF,ID,EX,WB]");
        Print(Table);
        System.out.println("");
        System.out.println("");
        Print(Table_Pipelined);
        System.out.println("");
        System.out.println("Time Of NonPipelined = " + TNP);
        System.out.println("Time Of Pipelined = " + TP);
        System.out.println("Speed Up = " + ((double) TNP / TP));
    }

    public static int[][] Apply_pipeline(int pip_Array[][], int whichInstruction/*Number of Instruction*/, int Length/*clock*/, int whichOperation) {
        if (whichOperation == 1/*IF*/) {
            for (int i = 0; i < pip_Array[0].length; i++) {
                if (pip_Array[0][i] == 0 & Length != 0) {
                    pip_Array[0][i] = whichInstruction;
                    Length--;
                }
            }
            return pip_Array;
        } else/*ID-EX-WB*/ {
            if (whichInstruction == 1/*First instruction*/) {
                for (int i = MAX_INDEX_AtValue(pip_Array[whichOperation - 2/*-1 for index , -1 row before*/], 1) + 1; i < pip_Array[0].length; i++) {
                    if (pip_Array[whichOperation - 1][i] == 0 & Length != 0) {
                        pip_Array[whichOperation - 1][i] = whichInstruction;
                        Length--;
                    }
                }
            } else/*Any instruction ≠ 1*/ {
                for (int M = (Math.max(MAX_INDEX(pip_Array[whichOperation - 1]), MAX_INDEX_AtValue(pip_Array[whichOperation - 2], whichInstruction)) + 1); M < pip_Array[0].length; M++) {
                    if (pip_Array[whichOperation - 1][M] == 0 & Length != 0) {
                        pip_Array[whichOperation - 1][M] = whichInstruction;
                        Length--;
                    }
                }
            }
        }
        return pip_Array;
    }

    public static int MAX_INDEX(int[] arr) {/*large index for large value in array*/
        int max = arr[0];
        int max_index = 0;
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] >= max) {
                max = arr[i];
                max_index = i;
            }
        }
        return max_index;
    }

    public static int MAX_INDEX_AtValue(int[] arr, int Target) {/*large index for Target value*/
        int max_index = 0;
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == Target) {
                max_index = i;
            }
        }
        return max_index;
    }

    public static void Print(int[][] arr) {
        /*Print 2D array*/
        for (int Row = 0; Row < arr.length; Row++) {
            System.out.print("[ ");
            for (int column = 0; column < arr[0].length; column++) {
                if (column != arr[0].length - 1) {
                    System.out.print(arr[Row][column] + " ,");
                } else {
                    System.out.print(arr[Row][column]);
                }
            }
            System.out.println("]");
        }
    }
    }

