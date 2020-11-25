Tutorials
===========================================


Implement your own problem
--------------------------------------
To make your problem compliant with the framework, your problem class have to extend the class *'moead_framework.problem.Problem'* 
and also implement the 3 required functions : 

- the fitness function *f* has 2 required parameters. The function must returns the objectif value of the solution 
  for the objective function_id in parameter.

.. code-block:: python
    
    def f(self, function_id, solution):
        ...
        return objective_value



- The function *generate_solution* allows to generate a solution. A solution is an object OneDimensionSolution that contains 
 the representation of the solution in the attribute solution and all fitness values in the F attribute. 
 
 .. note:: All components of the framework available in this version are only compatible with OneDimensionSolution. If you want manage new type of solutions, you have to adapt other components of the framework. 
 
 The *generate_solution* function takes in parameter the representation of the function and the boolean 'evaluate' 
 that determine if the solution save the fitness, the default value must be "True".

.. code-block:: python
    
        def generate_solution(self, array, evaluate=True):
            x = OneDimensionSolution(array)

            for j in range(self.function_numbers):
                if evaluate:
                    x.F.append(self.f(j, x.solution))
                else:
                    x.F.append(None)

            return x
  

- The *generate_random_solution* generates a solution randomly in the same way than the previous function.

.. code-block:: python

    def generate_random_solution(self, evaluate=True):
        return self.generate_solution(array=np.random.randint(0, 2, self.object_number).tolist()[:], evaluate=evaluate)




Implement your own algorithm with the framework
--------------------------------------------------------------------

Easy way : develop your own component and use it in parameter of an existing algorithm

Advanced way : extent an existing algorithm