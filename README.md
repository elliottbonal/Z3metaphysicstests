# Z3metaphysicstests
Computational experimentation for metaphysics using Z3 SMT Solver
!pip install z3-solver
from z3 import *

# Setting up the theorem solver
s = Solver() 

# Defining the domain of all valid types (T143)
T143 = DeclareSort('T143')

#Definining the Zplus function
Zplus = Function('Zplus', T143, RealSort())
s.add(Zplus(x) >= 0)

# Definitions of existence and non-existence
Exists_x = Zplus(x) > 0
NExists_x = Zplus(x) == 0

# Defining the existing entity as per the Axiom P
x = Const('x', T143)
Exists_x

# Defining the Totality of Things as the set of all things that exist
in_Psi = Zplus(x) > 0

# Defining the void as the set of all things that do not exist
in_Void = Zplus(x) == 0

# Definition of the Absolute as the set of all things that exist or do not exist
in_Absolute = Or(Exists_x, NExists_x)

# Defining the union of the Totality of Things and the Void
psi_union_void = Or(in_Psi, in_Void)

# Trying to Falsify the claim that the Absolute is the union of the Totality of Things and the Void
s.add(Not(ForAll([x], in_Absolute == psi_union_void))) 

result = s.check()
print("Z3 Result:")
if result == unsat:
    print("No counterexample possible, the theorem holds.")
else:
    print("The theorem fails, counterexample found.")
    print("Model showing inconsistency:")
    print(s.model())
