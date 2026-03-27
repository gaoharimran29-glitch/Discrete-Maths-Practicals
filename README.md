# 1. SET Class Implementation with Comprehensive Operations

```python
class SET:
    """
    A mathematical SET implementation supporting fundamental set operations.
    This class models a finite set with operations corresponding to standard
    set theory in discrete mathematics.
    """
    
    def __init__(self, elements):
        """
        Initialize a set with given elements.
        
        Parameters:
        elements (iterable): Collection of elements to form the set
        """
        # Remove duplicates to maintain set property (no repeated elements)
        # Convert to list for consistent iteration and indexing
        self.elements = list(set(elements))
    
    def is_member(self, element):
        """
        Check membership of an element in the set (∈ relation).
        
        Parameters:
        element: The element to test for membership
        
        Returns:
        bool: True if element ∈ set, False otherwise
        """
        return element in self.elements
    
    def powerset(self):
        """
        Generate the power set P(S) - the set of all subsets of S.
        
        Returns:
        list: List containing all subsets (each subset as a list)
        Computational Complexity: O(2^n) where n = |S|
        """
        # Start with empty set
        pset = [[]]
        
        # Iteratively build power set using binary inclusion principle
        for elem in self.elements:
            # For each existing subset, create a new subset including current element
            pset += [subset + [elem] for subset in pset]
        
        return pset
    
    def subset(self, other_set):
        """
        Determine if this set is a subset of another set (⊆ relation).
        
        Parameters:
        other_set (SET): The potential superset
        
        Returns:
        bool: True if self ⊆ other_set, False otherwise
        """
        # All elements of self must be in other_set
        return all(elem in other_set.elements for elem in self.elements)
    
    def union(self, other_set):
        """
        Compute union of two sets (A ∪ B).
        
        Parameters:
        other_set (SET): The set to union with
        
        Returns:
        SET: New set containing elements from both sets
        """
        # Union = all elements from both sets
        return SET(self.elements + other_set.elements)
    
    def intersection(self, other_set):
        """
        Compute intersection of two sets (A ∩ B).
        
        Parameters:
        other_set (SET): The set to intersect with
        
        Returns:
        SET: New set containing common elements
        """
        # Intersection = elements present in both sets
        return SET([elem for elem in self.elements if elem in other_set.elements])
    
    def complement(self, universal_set):
        """
        Compute complement relative to a universal set (Aᶜ = U - A).
        
        Parameters:
        universal_set (SET): The universal set U
        
        Returns:
        SET: New set containing elements in U but not in self
        """
        # Complement = elements in universal set minus elements in current set
        return SET([elem for elem in universal_set.elements if elem not in self.elements])
    
    def set_difference(self, other_set):
        """
        Compute set difference (A - B).
        
        Parameters:
        other_set (SET): The set to subtract
        
        Returns:
        SET: New set with elements in A but not in B
        """
        # Set difference = elements in self excluding those in other_set
        return SET([elem for elem in self.elements if elem not in other_set.elements])
    
    def symmetric_difference(self, other_set):
        """
        Compute symmetric difference (A Δ B = (A - B) ∪ (B - A)).
        
        Parameters:
        other_set (SET): The other set
        
        Returns:
        SET: New set containing elements in exactly one of the sets
        """
        # Symmetric difference = union of differences in both directions
        union_set = self.union(other_set)
        inter_set = self.intersection(other_set)
        return union_set.set_difference(inter_set)
    
    def cartesian_product(self, other_set):
        """
        Compute Cartesian product (A × B).
        
        Parameters:
        other_set (SET): The other set
        
        Returns:
        list: List of ordered pairs (a, b) where a ∈ A, b ∈ B
        """
        # Cartesian product = all possible ordered pairs
        return [(a, b) for a in self.elements for b in other_set.elements]
    
    def display(self):
        """
        Return the elements of the set.
        
        Returns:
        list: List of set elements
        """
        return self.elements


def menu_driven_set():
    """
    Interactive menu-driven interface for SET operations.
    Provides user-friendly access to all set operations.
    """
    print("=== SET OPERATIONS SIMULATOR ===")
    print("Enter elements of set A (space separated): ")
    a = list(map(str, input().split()))
    setA = SET(a)
    
    while True:
        print("\n" + "="*40)
        print("SET OPERATIONS MENU")
        print("="*40)
        print("1. Check Membership (is_member)")
        print("2. Generate Power Set")
        print("3. Check Subset Relation")
        print("4. Union of Two Sets")
        print("5. Intersection of Two Sets")
        print("6. Complement Relative to Universal Set")
        print("7. Set Difference (A - B)")
        print("8. Symmetric Difference (A Δ B)")
        print("9. Cartesian Product (A × B)")
        print("10. Exit Program")
        print("-"*40)
        
        try:
            choice = int(input("Enter your choice (1-10): "))
        except ValueError:
            print("Invalid input. Please enter a number.")
            continue
        
        if choice == 1:
            elem = input("Enter element to check for membership: ")
            result = setA.is_member(elem)
            print(f"Element '{elem}' is {'a member' if result else 'not a member'} of set A")
            
        elif choice == 2:
            pset = setA.powerset()
            print(f"Power Set of A (contains {len(pset)} subsets):")
            for i, subset in enumerate(pset):
                print(f"  Subset {i+1}: {subset}")
                
        elif choice == 3:
            b = list(map(str, input("Enter elements of set B (space separated): ").split()))
            setB = SET(b)
            result = setA.subset(setB)
            print(f"Set A = {setA.display()}")
            print(f"Set B = {setB.display()}")
            print(f"A ⊆ B: {result}")
            
        elif choice == 4:
            b = list(map(str, input("Enter elements of set B (space separated): ").split()))
            setB = SET(b)
            union_set = setA.union(setB)
            print(f"A ∪ B = {union_set.display()}")
            
        elif choice == 5:
            b = list(map(str, input("Enter elements of set B (space separated): ").split()))
            setB = SET(b)
            inter_set = setA.intersection(setB)
            print(f"A ∩ B = {inter_set.display()}")
            
        elif choice == 6:
            u = list(map(str, input("Enter elements of universal set U (space separated): ").split()))
            universal = SET(u)
            complement_set = setA.complement(universal)
            print(f"Universal Set U = {universal.display()}")
            print(f"A = {setA.display()}")
            print(f"Aᶜ (complement) = {complement_set.display()}")
            
        elif choice == 7:
            b = list(map(str, input("Enter elements of set B (space separated): ").split()))
            setB = SET(b)
            diff_set = setA.set_difference(setB)
            print(f"A - B = {diff_set.display()}")
            
        elif choice == 8:
            b = list(map(str, input("Enter elements of set B (space separated): ").split()))
            setB = SET(b)
            symdiff_set = setA.symmetric_difference(setB)
            print(f"A Δ B = {symdiff_set.display()}")
            
        elif choice == 9:
            b = list(map(str, input("Enter elements of set B (space separated): ").split()))
            setB = SET(b)
            product = setA.cartesian_product(setB)
            print(f"A × B (Cartesian Product):")
            for pair in product:
                print(f"  {pair}")
            print(f"Total ordered pairs: {len(product)}")
            
        elif choice == 10:
            print("Exiting SET Operations Simulator. Goodbye!")
            break
            
        else:
            print("Invalid choice. Please select 1-10.")
```

# 2. RELATION Class for Analyzing Binary Relations

```python
class RELATION:
    """
    A class to represent and analyze binary relations using matrix notation.
    The relation is defined on a finite set and represented as an adjacency matrix.
    """
    
    def __init__(self, matrix, set_elements):
        """
        Initialize a relation with its matrix representation.
        
        Parameters:
        matrix (list of lists): Adjacency matrix (n×n) where matrix[i][j]=1 
                               if (element_i, element_j) ∈ R
        set_elements (list): List of elements in the base set
        """
        self.matrix = matrix
        self.set_elements = set_elements
        self.n = len(matrix)  # Cardinality of the set
    
    def is_reflexive(self):
        """
        Check if the relation is reflexive.
        A relation R on set A is reflexive if ∀a∈A, (a,a)∈R.
        
        Returns:
        bool: True if relation is reflexive, False otherwise
        """
        # Check all diagonal entries - must be 1 for reflexivity
        for i in range(self.n):
            if self.matrix[i][i] != 1:
                return False
        return True
    
    def is_symmetric(self):
        """
        Check if the relation is symmetric.
        A relation R is symmetric if ∀a,b∈A, (a,b)∈R ⇒ (b,a)∈R.
        
        Returns:
        bool: True if relation is symmetric, False otherwise
        """
        # Check if matrix is symmetric across main diagonal
        for i in range(self.n):
            for j in range(self.n):
                if self.matrix[i][j] != self.matrix[j][i]:
                    return False
        return True
    
    def is_antisymmetric(self):
        """
        Check if the relation is antisymmetric.
        A relation R is antisymmetric if ∀a,b∈A, (a,b)∈R ∧ (b,a)∈R ⇒ a=b.
        
        Returns:
        bool: True if relation is antisymmetric, False otherwise
        """
        # For i≠j, cannot have both (i,j) and (j,i) in relation
        for i in range(self.n):
            for j in range(self.n):
                if i != j and self.matrix[i][j] == 1 and self.matrix[j][i] == 1:
                    return False
        return True
    
    def is_transitive(self):
        """
        Check if the relation is transitive.
        A relation R is transitive if ∀a,b,c∈A, (a,b)∈R ∧ (b,c)∈R ⇒ (a,c)∈R.
        
        Returns:
        bool: True if relation is transitive, False otherwise
        """
        # Check transitivity using matrix multiplication concept
        for i in range(self.n):
            for j in range(self.n):
                if self.matrix[i][j] == 1:  # If (i,j) in relation
                    for k in range(self.n):
                        if self.matrix[j][k] == 1 and self.matrix[i][k] == 0:
                            return False
        return True
    
    def is_equivalence(self):
        """
        Check if the relation is an equivalence relation.
        An equivalence relation must be reflexive, symmetric, and transitive.
        
        Returns:
        bool: True if equivalence relation, False otherwise
        """
        return (self.is_reflexive() and 
                self.is_symmetric() and 
                self.is_transitive())
    
    def is_partial_order(self):
        """
        Check if the relation is a partial order.
        A partial order must be reflexive, antisymmetric, and transitive.
        
        Returns:
        bool: True if partial order, False otherwise
        """
        return (self.is_reflexive() and 
                self.is_antisymmetric() and 
                self.is_transitive())
    
    def check_relation_type(self):
        """
        Determine the type of relation based on its properties.
        
        Returns:
        str: Classification of the relation
        """
        if self.is_equivalence():
            return "Equivalence Relation"
        elif self.is_partial_order():
            return "Partial Order Relation"
        else:
            return "General Binary Relation (not equivalence or partial order)"
    
    def display_properties(self):
        """
        Display all properties of the relation.
        """
        print("\n" + "="*50)
        print("RELATION ANALYSIS REPORT")
        print("="*50)
        print(f"Set Elements: {self.set_elements}")
        print(f"Matrix Representation:")
        for row in self.matrix:
            print("  " + " ".join(str(x) for x in row))
        print("\nProperties:")
        print(f"  Reflexive: {self.is_reflexive()}")
        print(f"  Symmetric: {self.is_symmetric()}")
        print(f"  Antisymmetric: {self.is_antisymmetric()}")
        print(f"  Transitive: {self.is_transitive()}")
        print(f"\nClassification: {self.check_relation_type()}")
        print("="*50)


# Example usage demonstration
def demonstrate_relation():
    """
    Demonstrate the RELATION class with examples.
    """
    print("=== RELATION CLASS DEMONSTRATION ===\n")
    
    # Example 1: Equivalence relation (equality modulo 2)
    print("Example 1: Equivalence Relation (Modulo 2)")
    matrix1 = [
        [1, 0, 1, 0],
        [0, 1, 0, 1],
        [1, 0, 1, 0],
        [0, 1, 0, 1]
    ]
    elements1 = [0, 1, 2, 3]
    rel1 = RELATION(matrix1, elements1)
    rel1.display_properties()
    
    # Example 2: Partial order (divisibility on {1,2,3,6})
    print("\nExample 2: Partial Order (Divisibility)")
    matrix2 = [
        [1, 1, 1, 1],  # 1 divides everything
        [0, 1, 0, 1],  # 2 divides 2 and 6
        [0, 0, 1, 1],  # 3 divides 3 and 6
        [0, 0, 0, 1]   # 6 divides 6
    ]
    elements2 = [1, 2, 3, 6]
    rel2 = RELATION(matrix2, elements2)
    rel2.display_properties()
```

# 3. Permutation Generator for Discrete Sequences

```python
def generate_permutations_without_repetition(elements):
    """
    Generate all permutations of a set without repetition.
    Uses the standard permutation concept from combinatorics.
    
    Parameters:
    elements (list): The set of distinct elements
    
    Returns:
    list: All possible permutations (n! total)
    """
    from itertools import permutations
    
    # Generate permutations using Python's efficient itertools
    perm_list = list(permutations(elements))
    
    print(f"\nGenerating permutations of {elements} without repetition:")
    print(f"Number of elements (n): {len(elements)}")
    print(f"Total permutations (n!): {len(perm_list)}")
    print("All permutations:")
    for i, perm in enumerate(perm_list, 1):
        print(f"  {i:3d}. {perm}")
    
    return perm_list


def generate_permutations_with_repetition(elements, length):
    """
    Generate all permutations with repetition (variations).
    Elements can be reused in each position.
    
    Parameters:
    elements (list): The set of available elements
    length (int): Length of each permutation
    
    Returns:
    list: All possible permutations with repetition (n^r total)
    """
    from itertools import product
    
    # Generate all r-length sequences from n elements with repetition
    perm_list = list(product(elements, repeat=length))
    
    print(f"\nGenerating {length}-length permutations of {elements} with repetition:")
    print(f"Number of elements (n): {len(elements)}")
    print(f"Length of permutations (r): {length}")
    print(f"Total permutations (n^r): {len(perm_list)}")
    print("Sample permutations (first 10):")
    for i, perm in enumerate(perm_list[:10], 1):
        print(f"  {i:3d}. {perm}")
    if len(perm_list) > 10:
        print(f"  ... and {len(perm_list)-10} more")
    
    return perm_list


def permutations_demonstration():
    """
    Demonstrate permutation generation with examples.
    """
    print("=== PERMUTATION GENERATOR DEMONSTRATION ===\n")
    
    # Example 1: Permutations without repetition
    digits = [1, 2, 3]
    generate_permutations_without_repetition(digits)
    
    # Example 2: Permutations with repetition
    print("\n" + "-"*50)
    generate_permutations_with_repetition(digits, 2)
    
    # Example 3: Larger example
    print("\n" + "-"*50)
    letters = ['A', 'B', 'C']
    print("Example with letters:")
    generate_permutations_without_repetition(letters)
```

# 4. Integer Partition Solutions via Brute Force Search

```python
def find_integer_solutions(n, C):
    """
    Find all non-negative integer solutions to:
    x₁ + x₂ + ... + xₙ = C
    
    This represents the classic "stars and bars" combinatorial problem.
    
    Parameters:
    n (int): Number of variables
    C (int): Constant sum (constrained to C ≤ 10 for practicality)
    
    Returns:
    list: All solution vectors as lists of length n
    """
    solutions = []
    
    # Validation
    if C > 10:
        print(f"Warning: C={C} exceeds recommended limit of 10.")
        print("Computation may be intensive.")
    
    def backtrack(current_solution, remaining_sum, position):
        """
        Recursive backtracking to explore solution space.
        
        Parameters:
        current_solution (list): Partial solution being built
        remaining_sum (int): Sum yet to be allocated
        position (int): Current variable index
        """
        # Base case: all positions filled
        if position == n:
            if remaining_sum == 0:
                solutions.append(current_solution.copy())
            return
        
        # Try all possible values for current variable (0 to remaining_sum)
        for value in range(remaining_sum + 1):
            current_solution.append(value)
            # Recurse with reduced sum and next position
            backtrack(current_solution, remaining_sum - value, position + 1)
            # Backtrack: remove last choice
            current_solution.pop()
    
    # Start backtracking from empty solution
    backtrack([], C, 0)
    
    return solutions


def analyze_solutions(n, C, solutions):
    """
    Analyze and display the found solutions.
    
    Parameters:
    n (int): Number of variables
    C (int): Constant sum
    solutions (list): List of solution vectors
    """
    print(f"\nSOLUTION ANALYSIS for {n} variables summing to {C}")
    print("="*60)
    print(f"Equation: x₁ + x₂ + ... + x_{n} = {C}")
    print(f"Number of solutions found: {len(solutions)}")
    
    # Theoretical count from stars and bars formula
    from math import comb
    theoretical_count = comb(C + n - 1, n - 1)
    print(f"Theoretical number of solutions (stars and bars): {theoretical_count}")
    
    if solutions:
        print(f"\nAll solutions (non-negative integer vectors):")
        for i, sol in enumerate(solutions, 1):
            # Format solution nicely
            sol_str = " + ".join(f"{val}" for val in sol)
            sum_str = " = ".join([sol_str, str(C)])
            print(f"  Solution {i:3d}: ({', '.join(str(v) for v in sol)}) → {sum_str}")
    else:
        print("No solutions found.")


def solve_linear_diophantine():
    """
    Interactive solver for the equation x₁ + x₂ + ... + xₙ = C
    """
    print("=== INTEGER SOLUTION FINDER ===")
    print("Solves: x₁ + x₂ + ... + xₙ = C")
    print("where all xᵢ are non-negative integers\n")
    
    try:
        n = int(input("Enter number of variables (n): "))
        C = int(input("Enter constant sum (C, recommended ≤ 10): "))
        
        if n <= 0 or C < 0:
            print("Error: n must be positive, C must be non-negative.")
            return
        
        solutions = find_integer_solutions(n, C)
        analyze_solutions(n, C, solutions)
        
    except ValueError:
        print("Invalid input. Please enter integers.")
```

# 5. Polynomial Function Evaluation Engine

```python
class Polynomial:
    """
    A class to represent and evaluate polynomial functions.
    Polynomials are stored in coefficient form: a₀ + a₁x + a₂x² + ... + aₙxⁿ
    """
    
    def __init__(self, coefficients):
        """
        Initialize polynomial with coefficients.
        
        Parameters:
        coefficients (list): List where coefficients[i] is coefficient of xⁱ
                           Example: [9, 2, 4] represents 9 + 2x + 4x²
        """
        self.coefficients = coefficients
        self.degree = len(coefficients) - 1
    
    def evaluate(self, x):
        """
        Evaluate polynomial at given x using Horner's method.
        Horner's method reduces multiplications and is numerically stable.
        
        Parameters:
        x (float or int): Value at which to evaluate polynomial
        
        Returns:
        float: f(x)
        """
        result = 0
        # Evaluate using standard polynomial form: Σ aᵢ * xⁱ
        for power, coeff in enumerate(self.coefficients):
            result += coeff * (x ** power)
        return result
    
    def evaluate_horner(self, x):
        """
        Alternative evaluation using Horner's method.
        More efficient for high-degree polynomials.
        
        Parameters:
        x (float or int): Value at which to evaluate polynomial
        
        Returns:
        float: f(x)
        """
        result = self.coefficients[-1]  # Start with highest degree coefficient
        
        # Apply Horner's scheme: ((...(aₙx + aₙ₋₁)x + ...)x + a₀)
        for coeff in reversed(self.coefficients[:-1]):
            result = result * x + coeff
        
        return result
    
    def display(self):
        """
        Display polynomial in human-readable form.
        """
        terms = []
        for power, coeff in enumerate(self.coefficients):
            if coeff != 0:
                if power == 0:
                    terms.append(f"{coeff}")
                elif power == 1:
                    terms.append(f"{coeff}x" if coeff != 1 else "x")
                else:
                    terms.append(f"{coeff}x^{power}" if coeff != 1 else f"x^{power}")
        
        if not terms:
            return "0"
        
        # Join terms with proper signs
        expression = terms[0]
        for term in terms[1:]:
            if term.startswith('-'):
                expression += f" - {term[1:]}"
            else:
                expression += f" + {term}"
        
        return expression
    
    def tabulate(self, start, end, step=1):
        """
        Generate table of polynomial values over a range.
        
        Parameters:
        start (int): Starting x value
        end (int): Ending x value (inclusive)
        step (int): Increment step
        
        Returns:
        dict: Mapping of x values to f(x)
        """
        results = {}
        x = start
        while x <= end:
            results[x] = self.evaluate(x)
            x += step
        return results


def polynomial_demo():
    """
    Demonstrate polynomial evaluation with examples.
    """
    print("=== POLYNOMIAL EVALUATION DEMONSTRATION ===\n")
    
    # Example polynomial: f(x) = 4x² + 2x + 9
    coeffs = [9, 2, 4]  # Constant, x, x² coefficients
    poly = Polynomial(coeffs)
    
    print(f"Polynomial: f(x) = {poly.display()}")
    print(f"Degree: {poly.degree}")
    print(f"Coefficient array: {poly.coefficients}")
    
    # Evaluate at specific point
    test_value = 5
    result_standard = poly.evaluate(test_value)
    result_horner = poly.evaluate_horner(test_value)
    
    print(f"\nEvaluation at x = {test_value}:")
    print(f"  Standard method: f({test_value}) = {result_standard}")
    print(f"  Horner's method: f({test_value}) = {result_horner}")
    print(f"  Verification: {coeffs[0]} + {coeffs[1]}*{test_value} + {coeffs[2]}*{test_value}² = {result_standard}")
    
    # Tabulate values
    print(f"\nValue table for x = 0 to 5:")
    print("-"*25)
    print("   x   |   f(x)")
    print("-"*25)
    table = poly.tabulate(0, 5)
    for x, fx in table.items():
        print(f"   {x:2d}   |   {fx:6.1f}")
    print("-"*25)
    
    return poly


def interactive_polynomial():
    """
    Interactive polynomial evaluator.
    """
    print("=== INTERACTIVE POLYNOMIAL EVALUATOR ===\n")
    
    print("Enter polynomial coefficients from lowest to highest degree.")
    print("Example: For 4x² + 2x + 9, enter: 9 2 4")
    
    try:
        coeff_input = input("Enter coefficients (space separated): ")
        coeffs = list(map(float, coeff_input.split()))
        
        if not coeffs:
            print("Error: No coefficients entered.")
            return
        
        poly = Polynomial(coeffs)
        print(f"\nPolynomial created: f(x) = {poly.display()}")
        
        while True:
            try:
                x_val = input("\nEnter x value to evaluate (or 'q' to quit): ")
                if x_val.lower() == 'q':
                    break
                
                x_val = float(x_val)
                result = poly.evaluate(x_val)
                print(f"f({x_val}) = {result}")
                
            except ValueError:
                print("Invalid input. Please enter a number.")
                
    except ValueError:
        print("Invalid coefficient format. Please enter numbers only.")
```

# 6. Complete Graph Verification System

```python
def is_complete_graph(adjacency_matrix):
    """
    Determine if a graph represented by adjacency matrix is complete.
    A complete graph Kₙ has n vertices with every pair of distinct vertices connected.
    
    Properties of complete graph adjacency matrix:
    1. All diagonal entries are 0 (no self-loops in simple graph)
    2. All off-diagonal entries are 1
    3. Matrix is symmetric for undirected graph
    
    Parameters:
    adjacency_matrix (list of lists): n×n adjacency matrix
    
    Returns:
    bool: True if graph is complete, False otherwise
    tuple: Additional information (n, edge_count, expected_edges)
    """
    n = len(adjacency_matrix)
    
    # Validate matrix is square
    for row in adjacency_matrix:
        if len(row) != n:
            raise ValueError("Adjacency matrix must be square")
    
    # Check for complete graph properties
    for i in range(n):
        for j in range(n):
            if i == j:
                # Diagonal should be 0 (no self-loops)
                if adjacency_matrix[i][j] != 0:
                    return False, (n, None, None)
            else:
                # Off-diagonal should be 1 (all vertices connected)
                if adjacency_matrix[i][j] != 1:
                    return False, (n, None, None)
    
    # Calculate edge count for complete graph
    # In undirected complete graph: edges = n(n-1)/2
    edge_count = sum(sum(row) for row in adjacency_matrix) // 2
    expected_edges = n * (n - 1) // 2
    
    return True, (n, edge_count, expected_edges)


def analyze_graph(matrix):
    """
    Perform comprehensive analysis of graph from adjacency matrix.
    
    Parameters:
    matrix (list of lists): Adjacency matrix
    """
    print("\n" + "="*60)
    print("GRAPH ANALYSIS")
    print("="*60)
    
    n = len(matrix)
    print(f"Number of vertices (n): {n}")
    
    # Display matrix
    print("\nAdjacency Matrix:")
    for i, row in enumerate(matrix):
        row_str = "  ".join(str(val) for val in row)
        print(f"  Vertex {i}: [{row_str}]")
    
    # Check if complete
    is_complete, info = is_complete_graph(matrix)
    
    if is_complete:
        n_vertices, actual_edges, expected_edges = info
        print(f"\n✓ This is a COMPLETE GRAPH K_{n_vertices}")
        print(f"  Actual edges: {actual_edges}")
        print(f"  Expected edges for K_{n_vertices}: {expected_edges}")
        print(f"  Edge verification: {actual_edges} == {expected_edges}")
        
        # Properties of complete graph
        print("\nProperties of Complete Graph:")
        print(f"  • Every vertex has degree {n_vertices-1}")
        print(f"  • Total edges: n(n-1)/2 = {n_vertices}({n_vertices-1})/2 = {expected_edges}")
        print(f"  • Diameter: 1 (any two vertices are directly connected)")
        print(f"  • Regular: Yes, all vertices have same degree")
        
    else:
        print(f"\n✗ This is NOT a complete graph")
        print("  Reasons (checking):")
        
        # Diagnose why not complete
        missing_edges = []
        for i in range(n):
            for j in range(i+1, n):  # Check only upper triangle for undirected
                if matrix[i][j] == 0:
                    missing_edges.append((i, j))
        
        if missing_edges:
            print(f"  • Missing edges: {missing_edges[:5]}", end="")
            if len(missing_edges) > 5:
                print(f" ... and {len(missing_edges)-5} more")
            else:
                print()
        
        # Count actual edges
        edge_count = sum(sum(row) for row in matrix) // 2
        expected_complete_edges = n * (n - 1) // 2
        print(f"  • Actual edges: {edge_count}")
        print(f"  • Edges needed for completeness: {expected_complete_edges}")
        print(f"  • Missing edge count: {expected_complete_edges - edge_count}")
    
    print("="*60)


def graph_examples():
    """
    Provide examples of complete and non-complete graphs.
    """
    print("=== COMPLETE GRAPH VERIFICATION EXAMPLES ===\n")
    
    # Example 1: Complete graph K₃
    print("Example 1: Complete Graph K₃ (Triangle)")
    k3 = [
        [0, 1, 1],
        [1, 0, 1],
        [1, 1, 0]
    ]
    analyze_graph(k3)
    
    # Example 2: Incomplete graph (path of length 2)
    print("\n\nExample 2: Incomplete Graph (P₃ - Path with 3 vertices)")
    p3 = [
        [0, 1, 0],
        [1, 0, 1],
        [0, 1, 0]
    ]
    analyze_graph(p3)
    
    # Example 3: Complete graph K₄
    print("\n\nExample 3: Complete Graph K₄")
    k4 = [
        [0, 1, 1, 1],
        [1, 0, 1, 1],
        [1, 1, 0, 1],
        [1, 1, 1, 0]
    ]
    analyze_graph(k4)


def interactive_graph_check():
    """
    Interactive tool for checking if a graph is complete.
    """
    print("=== INTERACTIVE GRAPH COMPLETENESS CHECKER ===\n")
    
    print("Enter adjacency matrix row by row.")
    print("Example for K₃ (3 vertices, all connected):")
    print("  Row 1: 0 1 1")
    print("  Row 2: 1 0 1")
    print("  Row 3: 1 1 0")
    print()
    
    try:
        n = int(input("Enter number of vertices: "))
        if n <= 0:
            print("Number of vertices must be positive.")
            return
        
        matrix = []
        print(f"\nEnter {n} rows, each with {n} numbers (0 or 1):")
        
        for i in range(n):
            while True:
                row_input = input(f"Row {i+1}: ")
                values = row_input.split()
                
                if len(values) != n:
                    print(f"Error: Expected {n} values, got {len(values)}")
                    continue
                
                try:
                    row = [int(val) for val in values]
                    if any(val not in [0, 1] for val in row):
                        print("Error: All values must be 0 or 1")
                        continue
                    
                    matrix.append(row)
                    break
                    
                except ValueError:
                    print("Error: All values must be integers")
        
        analyze_graph(matrix)
        
    except ValueError:
        print("Invalid input. Please enter integers only.")
```

# 7. Directed Graph Degree Analysis Tool

```python
def analyze_directed_graph(adjacency_matrix):
    """
    Analyze in-degree and out-degree of each vertex in a directed graph.
    
    In-degree: Number of edges coming INTO a vertex
    Out-degree: Number of edges going OUT FROM a vertex
    
    Parameters:
    adjacency_matrix (list of lists): n×n matrix where
                                     matrix[i][j] = 1 if edge from i to j
    
    Returns:
    dict: Degree information for each vertex
    """
    n = len(adjacency_matrix)
    
    # Initialize degree arrays
    in_degrees = [0] * n
    out_degrees = [0] * n
    total_edges = 0
    
    # Calculate degrees
    for i in range(n):
        for j in range(n):
            if adjacency_matrix[i][j] == 1:
                out_degrees[i] += 1
                in_degrees[j] += 1
                total_edges += 1
    
    # Prepare results
    results = {
        'vertices': n,
        'edges': total_edges,
        'degrees': []
    }
    
    for v in range(n):
        results['degrees'].append({
            'vertex': v,
            'in_degree': in_degrees[v],
            'out_degree': out_degrees[v],
            'total_degree': in_degrees[v] + out_degrees[v]
        })
    
    return results


def display_degree_analysis(results):
    """
    Display comprehensive degree analysis of directed graph.
    
    Parameters:
    results (dict): Degree analysis results from analyze_directed_graph
    """
    print("\n" + "="*70)
    print("DIRECTED GRAPH DEGREE ANALYSIS")
    print("="*70)
    
    print(f"Graph Summary:")
    print(f"  Number of vertices: {results['vertices']}")
    print(f"  Number of directed edges: {results['edges']}")
    
    print("\n" + "-"*70)
    print("Vertex-wise Degree Analysis:")
    print("-"*70)
    print(f"{'Vertex':^8} | {'In-Degree':^10} | {'Out-Degree':^11} | {'Total Degree':^12}")
    print("-"*70)
    
    for deg_info in results['degrees']:
        v = deg_info['vertex']
        indeg = deg_info['in_degree']
        outdeg = deg_info['out_degree']
        total = deg_info['total_degree']
        
        print(f"{v:^8} | {indeg:^10} | {outdeg:^11} | {total:^12}")
    
    print("-"*70)
    
    # Calculate statistics
    in_degrees = [d['in_degree'] for d in results['degrees']]
    out_degrees = [d['out_degree'] for d in results['degrees']]
    
    print("\nDegree Statistics:")
    print(f"  Average in-degree: {sum(in_degrees)/len(in_degrees):.2f}")
    print(f"  Average out-degree: {sum(out_degrees)/len(out_degrees):.2f}")
    print(f"  Maximum in-degree: {max(in_degrees)} at vertex {in_degrees.index(max(in_degrees))}")
    print(f"  Maximum out-degree: {max(out_degrees)} at vertex {out_degrees.index(max(out_degrees))}")
    
    # Check for special vertices
    sources = [v for v, deg in enumerate(in_degrees) if deg == 0]
    sinks = [v for v, deg in enumerate(out_degrees) if deg == 0]
    
    if sources:
        print(f"  Source vertices (in-degree 0): {sources}")
    if sinks:
        print(f"  Sink vertices (out-degree 0): {sinks}")
    
    # Verify handshaking lemma for directed graphs
    # Sum of in-degrees = Sum of out-degrees = Number of edges
    total_indeg = sum(in_degrees)
    total_outdeg = sum(out_degrees)
    
    print("\nHandshaking Lemma Verification (for directed graphs):")
    print(f"  Sum of all in-degrees: {total_indeg}")
    print(f"  Sum of all out-degrees: {total_outdeg}")
    print(f"  Number of edges: {results['edges']}")
    print(f"  Verification: {total_indeg} == {total_outdeg} == {results['edges']}: "
          f"{total_indeg == total_outdeg == results['edges']}")
    
    print("="*70)


def directed_graph_examples():
    """
    Demonstrate directed graph analysis with examples.
    """
    print("=== DIRECTED GRAPH DEGREE ANALYSIS EXAMPLES ===\n")
    
    # Example 1: Simple directed cycle
    print("Example 1: Directed Cycle C₃")
    cycle_3 = [
        [0, 1, 0],  # 0 → 1
        [0, 0, 1],  # 1 → 2
        [1, 0, 0]   # 2 → 0
    ]
    results1 = analyze_directed_graph(cycle_3)
    display_degree_analysis(results1)
    
    # Example 2: Directed acyclic graph (DAG)
    print("\n\nExample 2: Directed Acyclic Graph (DAG)")
    dag = [
        [0, 1, 1, 0],  # 0 → 1, 0 → 2
        [0, 0, 0, 1],  # 1 → 3
        [0, 0, 0, 1],  # 2 → 3
        [0, 0, 0, 0]   # 3 has no outgoing edges
    ]
    results2 = analyze_directed_graph(dag)
    display_degree_analysis(results2)
    
    # Example 3: Complete directed graph (tournament)
    print("\n\nExample 3: Tournament Graph (Complete Directed)")
    tournament = [
        [0, 1, 1],  # 0 beats 1 and 2
        [0, 0, 1],  # 1 beats 2
        [0, 0, 0]   # 2 loses to all
    ]
    results3 = analyze_directed_graph(tournament)
    display_degree_analysis(results3)


def interactive_degree_analysis():
    """
    Interactive tool for directed graph degree analysis.
    """
    print("=== INTERACTIVE DIRECTED GRAPH ANALYZER ===\n")
    
    print("Enter adjacency matrix for a directed graph.")
    print("For n vertices, enter n rows of n numbers (0 or 1).")
    print("matrix[i][j] = 1 means there's an edge FROM vertex i TO vertex j\n")
    
    try:
        n = int(input("Enter number of vertices: "))
        if n <= 0:
            print("Number of vertices must be positive.")
            return
        
        matrix = []
        print(f"\nEnter {n} rows, each with {n} numbers:")
        
        for i in range(n):
            while True:
                row_input = input(f"Row {i} (outgoing from vertex {i}): ")
                values = row_input.split()
                
                if len(values) != n:
                    print(f"Error: Expected {n} values, got {len(values)}")
                    continue
                
                try:
                    row = [int(val) for val in values]
                    if any(val not in [0, 1] for val in row):
                        print("Error: All values must be 0 or 1")
                        continue
                    
                    matrix.append(row)
                    break
                    
                except ValueError:
                    print("Error: All values must be integers")
        
        results = analyze_directed_graph(matrix)
        display_degree_analysis(results)
        
    except ValueError:
        print("Invalid input. Please enter integers only.")


# Main execution demonstration
if __name__ == "__main__":
    print("DISCRETE STRUCTURES PRACTICAL IMPLEMENTATIONS")
    print("="*70)
    
    # Uncomment any section to run specific demonstrations
    
    # 1. SET Operations
    # menu_driven_set()
    
    # 2. Relation Analysis
    # demonstrate_relation()
    
    # 3. Permutations
    # permutations_demonstration()
    
    # 4. Integer Solutions
    # solve_linear_diophantine()
    
    # 5. Polynomial Evaluation
    # polynomial_demo()
    # interactive_polynomial()
    
    # 6. Complete Graph Check
    # graph_examples()
    # interactive_graph_check()
    
    # 7. Directed Graph Degrees
    # directed_graph_examples()
    # interactive_degree_analysis()
    
    print("\nAll implementations are complete and ready for use.")
    print("Uncomment the desired demonstration in the main section.")
```
