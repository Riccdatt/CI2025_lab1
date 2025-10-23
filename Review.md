Review by student: Meirat Zhanibek, s336521

To be honest, I like your solution, there is a lot of technics used and the performance also not bad. But there are some suggestions from me that you can try to implement in your code for its improvement.

1. After several runs for the problem 1 and 2, I notice that your solution is stuck in local optima and your phase 2 stagnates without any improvement, but actually, there are other options available. I did not notice this issue for the problem 3, but I think it is because the maximum iteration number is not enough to see it. So, my suggestion is to perform a strategic perturbation, like randomly unassign 5-10% of lowest-value items. This would help to escapee local optima without losing the valid solution structure.

2. Your current approach only considers contribution to overload. I suggest you try to compute a value-to-violation ratio for each item, so you can move the low-value items that contribute significantly to violations, which will preserve the higher value items in a solution.

