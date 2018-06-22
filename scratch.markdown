Would be interesting as an appendix to provide some code. https://stackoverflow.com/questions/5889142/python-numpy-scipy-finding-the-null-space-of-a-matrix

# [Given an unbalanced chemical equation, is it possible to determine the largest possible coefficient that could be used in the balanced equation?](https://chemistry.stackexchange.com/q/60038/194)

tags: equilibrium computational-chemistry stoichiometry

I am writing a program to balance chemical equations, and would love to know if there is an algebraic way to determine the greatest possible coefficient that I would need to check.

## Answer

#### In short

Just use matrices.

##### In long

Consider this equation to be balanced:

$$\ce{CuSCN + KIO3 + HCl -> CuSO4 + KCl + HCN + ICl + H2O}$$

([source with spoiler](https://answers.yahoo.com/question/index?qid=20080530150323AA8tQMu))

We are trying to solve this equation:

$$\ce{aCuSCN + bKIO3 + cHCl -> dCuSO4 + eKCl + fHCN + gICl + hH2O}$$

Now, we move every term to the left hand side (warning: handwavey chemistry):

$$\ce{aCuSCN + bKIO3 + cHCl - dCuSO4 - eKCl - fHCN - gICl - hH2O = 0}$$

Transform into matrix form (the element names are just for reference):

$$
\begin{pmatrix}
\ce{Cu} \\ \ce{S} \\ \ce{C} \\ \ce{N} \\ \ce{K} \\ \ce{I} \\ \ce{O} \\ \ce{H} \\ \ce{Cl}
\end{pmatrix}
\begin{pmatrix}
1 & 0 & 0 & -1 &  0 &  0 &  0 &  0 \\
1 & 0 & 0 & -1 &  0 &  0 &  0 &  0 \\
1 & 0 & 0 &  0 &  0 & -1 &  0 &  0 \\
1 & 0 & 0 &  0 &  0 & -1 &  0 &  0 \\
0 & 1 & 0 &  0 & -1 &  0 &  0 &  0 \\
0 & 1 & 0 &  0 &  0 &  0 & -1 &  0 \\
0 & 3 & 0 & -4 &  0 &  0 &  0 & -1 \\
0 & 0 & 1 &  0 &  0 & -1 &  0 & -2 \\
0 & 0 & 1 &  0 & -1 &  0 & -1 &  0
\end{pmatrix}
\begin{pmatrix}
a \\ b \\ c \\ d \\ e \\ f \\ g \\ h
\end{pmatrix}
=
\begin{pmatrix}
0 \\ 0 \\ 0 \\ 0 \\ 0 \\ 0 \\ 0 \\ 0 \\ 0 \\
\end{pmatrix}
$$

Notice that the biggest matrix is **not a square**. You must transform it to a square. If the elements are in excess (just like this case), add variables; otherwise, add elements. Set the extra variables/elements to zero:

$$
\begin{pmatrix}
\ce{Cu} \\ \ce{S} \\ \ce{C} \\ \ce{N} \\ \ce{K} \\ \ce{I} \\ \ce{O} \\ \ce{H} \\ \ce{Cl}
\end{pmatrix}
\begin{pmatrix}
1 & 0 & 0 & -1 &  0 &  0 &  0 &  0 & 0 \\
1 & 0 & 0 & -1 &  0 &  0 &  0 &  0 & 0 \\
1 & 0 & 0 &  0 &  0 & -1 &  0 &  0 & 0 \\
1 & 0 & 0 &  0 &  0 & -1 &  0 &  0 & 0 \\
0 & 1 & 0 &  0 & -1 &  0 &  0 &  0 & 0 \\
0 & 1 & 0 &  0 &  0 &  0 & -1 &  0 & 0 \\
0 & 3 & 0 & -4 &  0 &  0 &  0 & -1 & 0 \\
0 & 0 & 1 &  0 &  0 & -1 &  0 & -2 & 0 \\
0 & 0 & 1 &  0 & -1 &  0 & -1 &  0 & 0
\end{pmatrix}
\begin{pmatrix}
a \\ b \\ c \\ d \\ e \\ f \\ g \\ h \\ i
\end{pmatrix}
=
\begin{pmatrix}
0 \\ 0 \\ 0\\ 0 \\ 0 \\ 0 \\ 0 \\ 0 \\ 0 \\
\end{pmatrix}
$$

**Note**: $i$ must be zero.

Now you can use any algorithm for solving homogeneous square matrix equations to continue.

### Comments

There is no guarantee that any solver gives exact integer multiples and if the matrix is singular (as in your example) simply scaling the result vector won't necessarily work either. You will actually need to use an integer matrix solver. A Gaussian elimination with pivoting that only uses integer multipliers for instance should do this correctly. The smallest common multiplier of all matrix entries should provide the upper bound.

Also don't pad matrices to make them square. It's unnecessary.

### Final answer

$$\ce{4 CuSCN + 7 KIO3 + 14 HCl -> 4 CuSO4 + 7 KCl + 4 HCN + 7 ICl + 5 H2O}$$

what about

$$\ce{5 CuSCN + 7 KIO3 + 7 HCl -> 5 CuSO4 + 7 KCl + 5 HCN + 7 ICl + H2O}$$
