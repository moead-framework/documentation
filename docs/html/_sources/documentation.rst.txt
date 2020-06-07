
Components of MOEA/D framework
===========================================


Problems 
--------------------------------------

========================================= ======================================================= ===================================================================
Common Name                               Name in the framework                                   Comments
========================================= ======================================================= ===================================================================
rho MNK-Landscapes                        :class:`moead_framework.problem.combinatorial.rmnk`     The problem needs an instance file in parameter
Multi-objective Knapsack                  :class:`moead_framework.problem.combinatorial.knapsack` The problem needs an instance file in parameter
Zdt1                                      :class:`moead_framework.problem.numerical.zdt`          The problem needs the number of variables in parameter
========================================= ======================================================= ===================================================================


Some problems need an instance file, you can find some files in https://github.com/moead-framework/data/tree/master/problem

   
Three functions are available in a problem : 

=================================================== ===================================================================
Function                                            Comments
=================================================== ===================================================================
problem.generate_random_solution(evaluate)          Generate a random solution. The parameter `evaluate` is a boolean to specify if the solution have to be evaluated. The default value is True.
problem.generate_solution(x, evaluate)              Convert the list `x` to a Solution object. The parameter `evaluate` is a boolean to specify if the solution have to be evaluated. The default value is True.
problem.f(i, solution)                              Return the fitness value of the function `i`.
=================================================== ===================================================================

A solution is a `OneDimensionSolution` object. This object save the array in the attribute 
`OneDimensionSolution.solution`, the fitness is save in the array `OneDimensionSolution.F` if the  
parameter `evaluate` is set to True.

Example with the problem rho MNK-Landscapes:

.. code-block:: python

    In [1]: from moead_framework.problem.combinatorial.rmnk import Rmnk

    # the file is available here : https://github.com/moead-framework/data/blob/master/problem/RMNK/Instances/rmnk_0_2_20_1_0.dat
    # rmnk instance with parameters rho=0, m=2, n=20, k=1 and a seed of 0
    In [2]: file_rmnk = "rmnk_0_2_20_1_0.dat"  
    In [3]: rmnk = Rmnk(instance_file=file_rmnk) 

    # Generate a random solution
    In [4]: solution = rmnk.generate_random_solution()

    # Generate a solution with predefined values
    In [5]: solution = rmnk.generate_solution([0,1,1,1,0,1,0,0,1,0])

    # evaluate the solution for the function f0 and f1 of the problem
    In [6]: f0 = rmnk.f(0, solution)
    In [7]: f1 = rmnk.f(1, solution)

In practice, it is not necessary to evaluate solutions in our algorithm because when a new solution is generated with our functions, 
the solution is evaluated (if the parameter is not set to False) and all fitness values are accessible in solution.F

.. code-block:: python
    
    In [8]: array_of_fitness = solution.F
    In [9]: f1 = solution.F[1]


Algorithms
--------------------------------------

========================================= ================================================================== ===================================================================
Common Name                               Name in the framework                                              Comments
========================================= ================================================================== ===================================================================
Original MOEA/D (combinatorial)           :class:`moead_framework.algorithm.combinatorial.moead`             The original algorithm for combinatorial optimization
Original MOEA/D (continue/numerical)      :class:`moead_framework.algorithm.numerical.moead`                 The original algorithm for numerical optimization
MOEA/D with delta and nr                  :class:`moead_framework.algorithm.combinatorial.moead_delta_nr`    Variant with parameters delta & nr of MOEA/D-DE
MOEA/D-DRA                                :class:`moead_framework.algorithm.combinatorial.moead_dra`         Variant with a dynamic ressource allocation
========================================= ================================================================== ===================================================================

Each algorithm can be executed with the `run()` function. This function return all non dominated solutions found by the 
algorithm. Example : 

.. code-block:: python
    
    moead = Moead(problem=rmnk,
              max_evaluation = number_of_evaluations,
              number_of_objective=number_of_objective,
              number_of_weight=number_of_weight,
              number_of_weight_neighborhood=number_of_weight_neighborhood,
              number_of_crossover_points=number_of_crossover_points,
              weight_file=weight_file,
              )

    non_dominated_solutions = moead.run(g=Tchebycheff())


Aggregation function
--------------------------------------
========================================= ========================================= 
Common Name                               Name in the framework                    
========================================= ========================================= 
Weighted Sum                              :class:`moead_framework.aggregation.weighted_sum`   
Tchebycheff                               :class:`moead_framework.aggregation.tchebycheff`    
========================================= ========================================= 

The aggregation function is used in MOEA/D to decompose the multi-objective problem into several mono-objective problems. 
The two main functions used are the Weighted Sum and the Tchebycheff function. In our framework, the aggregation function
is a parameter of the function `run()` of the algorithm.

.. code-block:: python

    non_dominated_solutions = moead.run(g=Tchebycheff())


Genetic operator
--------------------------------------
A genetic operator is a component used in genetics algorithms to generate offspring by 
using characteristics of parents solutions. In the framework, these operators are used in the component offspring_generator.

========================================= ============================================================================================= ===================================================================
Common Name                               Name in the framework                                                                         Reference
========================================= ============================================================================================= ===================================================================
Multi-point combinatorial crossover       :class:`moead_framework.core.genetic_operator.combinatorial.crossover`                        [0]
Binary mutation                           :class:`moead_framework.core.genetic_operator.combinatorial.mutation`                         [0]
Differential Evolution Crossover          :class:`moead_framework.core.genetic_operator.numerical.differential_evolution_crossover`     [0]
Polynomial mutation                       :class:`moead_framework.core.genetic_operator.numerical.polynomial_mutation`                  [0]
========================================= ============================================================================================= ===================================================================


Offspring generator
--------------------------------------
The offspring generator is the component used to generate new offspring. It defines the method to choose parents solutions
and then uses the genetic operator to generate the offspring.

========================================= ========================================= 
Common Name                               Name in the framework                    
========================================= ========================================= 
Two random solutions                      :class:`moead_framework.core.offspring_generator.two_random_parents`    
One random and current solution           :class:`moead_framework.core.offspring_generator.one_random_and_current_parents`    
Two random and current solution           :class:`moead_framework.core.offspring_generator.two_random_and_current_parents`    
========================================= ========================================= 