## Python OOP Assignment
Q1. What is the purpose of Python's OOP?
> Python OOP concept helps us to solve complex problems by using objects(Similar to real world)
> OOP has other advantages like Encapsulation, Ploymorphism, Abstraction, Inheritance, etc.

Q2. Where does an inheritance search look for an attribute?
> In an inheritance the attribute is first serached in the class the object was created. Later it will search in the upper super classes.

Q3. How do you distinguish between a class object and an instance object?
> Instance object is always associated with self keyword & it is bound yo a particular object.
> Class object is bound to a class & hence self keyword is not used.

Q4. What makes the first argument in a class’s method function special?
> In a class the first argument is self keyword. It is nothing but a refernce to the object who called that method.

Q5. What is the purpose of the init method?
> \__init\__() method in a class is a constructor of that calss
> It gets called as soon as an object is created.

Q6. What is the process for creating a class instance?
> Class instance can be created anywhere in the body of class.
> We can declare & define it similar to any other variable declartion without the self keyword

Q7. What is the process for creating a class?
```
# Creating blank class
class Data():
	pass
	
# Creating a class with a constructor
class Data()
	def __init__(self):
		print("Welcome to the Data class")
```

Q8. How would you define the superclasses of a class?

Q9. What is the relationship between classes and modules?

Q10. How do you make instances and classes?

Q11. Where and how should be class attributes created?

Q12. Where and how are instance attributes created?

Q13. What does the term "self" in a Python class mean?

Q14. How does a Python class handle operator overloading?

Q15. When do you consider allowing operator overloading of your classes?

Q16. What is the most popular form of operator overloading?

Q17. What are the two most important concepts to grasp in order to comprehend Python OOP code?

Q18. Describe three applications for exception processing.

Q19. What happens if you don't do something extra to treat an exception?

Q20. What are your options for recovering from an exception in your script?

Q21. Describe two methods for triggering exceptions in your script.

Q22. Identify two methods for specifying actions to be executed at termination time, regardless of  
whether or not an exception exists.

Q23. What is the purpose of the try statement?

Q24. What are the two most popular try statement variations?

Q25. What is the purpose of the raise statement?

Q26. What does the assert statement do, and what other statement is it like?

Q27. What is the purpose of the with/as argument, and what other statement is it like?

Q28. What are *args, **kwargs?

Q29. How can I pass optional or keyword parameters from one function to another?

Q30. What are Lambda Functions?

Q31. Explain Inheritance in Python with an example?

Q32. Suppose class C inherits from classes A and B as class C(A,B).Classes A and B both have their own versions of method func(). If we call func() from an object of 
class C, which version gets invoked?

Q33. Which methods/functions do we use to determine the type of instance and inheritance?

Q34.Explain the use of the 'nonlocal' keyword in Python.

Q35. What is the global keyword?
