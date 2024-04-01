import sympy as sp #Use to solve symbolic functions 
import matplotlib.pyplot as plt

def solve_and_graph_specific_problem(input_file, output_file, problem_number):
    try:
        with open(input_file, 'r', encoding='utf-8') as input_f: 
            expressions = input_f.readlines()
            # Extracts expression so that the problem can be specified more
            expr_str = expressions[problem_number - 1].strip()
            expr = sp.sympify(expr_str)
            # Defines the variable
            x = sp.Symbol('x')
            # Generate x values from 1 to 50 (51 including the title)
            x_values = list(range(1, 51))
            #Evaluation or substitution of expression for each x value
            y_values = [expr.subs(x, x_val) for x_val in x_values]
            #Define color for the line para to sa specific prob
            color = plt.cm.gist_rainbow(1)

            #Ploting the graph using matplotlib
            plt.plot(x_values, y_values, label=f"Problem {problem_number}", color=color)
            plt.title(f"Graph of {sp.latex(expr)}") 
            plt.xlabel('x')
            plt.ylabel('y')
            plt.grid(True)
            plt.show()

            # Write solutions to the output file
            with open(output_file, 'w', encoding='utf-8') as output_f:
                output_f.write(f"Solutions for Problem {problem_number}:\n")
                for x_val, y_val in zip(x_values, y_values):
                    output_f.write(f"x = {x_val}, y = {y_val}\n")
    #For error handling 
    except FileNotFoundError:
        print("Input file not found.")
    except Exception as e:
        print("An error occurred:", str(e))
#For graphs
def solve_and_graph_all_problems(input_file, output_file):
    try:
        with open(input_file, 'r', encoding='utf-8') as input_f:
            expressions = input_f.readlines()
            x = sp.Symbol('x')
            x_values = list(range(1, 51))
            #Plot the graph for each expression
            for problem_number, expr_str in enumerate(expressions, 1):
                expr = sp.sympify(expr_str.strip())
                # Evaluate the expression for each x value
                y_values = [expr.subs(x, x_val) for x_val in x_values]
                # Define color for the line for all expr
                color = plt.cm.gist_rainbow(problem_number / len(expressions))
                # Plot the graph
                plt.plot(x_values, y_values, label=f"${sp.latex(expr)}$")#label for plot of all expr using latex
                # Writes solutions to the output file
                with open(output_file, 'a', encoding ='utf-8') as output_f:
                    output_f.write(f"\nSolutions for Problem {problem_number}:\n")
                    for x_val, y_val in zip(x_values, y_values):
                        output_f.write(f"x = {x_val}, y = {y_val}\n")
            plt.title("Graphs of All Problems")
            plt.xlabel('x')
            plt.ylabel('y')
            plt.legend()
            plt.grid(True)
            plt.show()
            
    except FileNotFoundError:
        print("Input file not found.")
    except Exception as e:
        print("An error occurred:", str(e))
#Files for read&write
input_file = 'problems_list.txt'
output_file = 'C:/Users/LENOVO/Documents/Python DSA/problem_solutions.txt'  # Specify the output file path
print('Problems:')
expressions = {
    '1': 'x² + 7x +2',
    '2': '3x + 2',
    '3': 'x²',
    '4': 'x³',
    '5': 'x⁵',
    '6': 'x³ + 2x² + x + 10',
    '7': 'x⁴ -3x³ + 2x² - x + 11',
    '8': 'Sin(x)',
    '9': 'Cos(x)',
    '10': '4x⁴ + x³ - 2x² + 100'
}
for key, value in expressions.items():
    print(key + ". " + value)

print("Choose an option:")
print("1. Solve and graph a specific problem")
print("2. Graph all given problems")
option = int(input("Enter your choice(1/2): "))

if option == 1:
    print("Problems:")
    for key, value in expressions.items():
        print(key + ". " + value)
    # Loop to ask the user to solve another problem  
    while True:
        problem_number = int(input("Enter the problem number to be solved and graphed: "))
        solve_and_graph_specific_problem(input_file, output_file, problem_number)

        try_again = input("Do you want to solve another problem? (Yes/No): ").lower()
        if try_again != 'Yes':
            break
elif option == 2:
    solve_and_graph_all_problems(input_file, output_file)
elif option == 3:
    print("Exiting...")
else:
    print("Invalid option. Kindly Choose 1, 2, or 3.")
