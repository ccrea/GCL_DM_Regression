# GCL_DM_Regression
I wrote an R Program to estimate the parameters of a Grouped Conditional Logit (GCL) and Dirichlet-multinomial (DM) regression model in the context of modelling mutualistic networks.
The program consists of a main function named "dirmultreg" and a series of utility functions.  The optimization is performed using R's built-in "optim" function.

Here I provide an example of how to use the "dirmultreg" function to analyze the artificial plant and pollinator data sets.  

Copy and paste the R program into a .R script file and load the file into R so that the dirmultreg function is available for use:
1.  Launch the R console
2. File -> New Script.  An untitled R Editor window will open.
3. Paste R program into R Editor.
4. File -> Save.  A Save script as window will open.  Navigate to the folder you would like to save this script file, give script a name.  Note under “Save as type”, it will default to “R files (*.R)”.  Do not change this file format.
5. Load and execute the script file with the R commands using the source function:
source(myfile.R), where the myfile.R argument is a character string giving the path name and file name.  

The data needs to be in this format:
plant	poll	Y	x1	x2	x3	x4	z1
1	1	0	1	1	0	0.068115	0.000987
2	1	0	1	0	0	0.062685	0.177443
3	1	1	1	0	1	0.129319	0.001481
4	1	2	1	0	0	0.003455	0.023445
5	1	1	1	0	1	0.017029	0.047631
6	1	2	1	0	0	0.007404	0.08539
7	1	1	1	0	1	0.293189	0.000247
8	1	3	0	0	0	0.031836	0.000247
9	1	1	0	1	0	0.019743	0.068115
10	1	0	1	0	1	0.000987	0.062685

In this example, plant and poll are group identifiers for plant and pollinator species, respectively, Y is the count or number of interactions recorded for that plant and pollinator pair, x1 – x4 are the associated predictor or covariate values for the plant and pollinator pairs, and z1 is the associated pollinator species covariate used to model dispersion as a function, if needed.
The dirmultreg function fits a GCL, which corresponds to a DM model with no dispersion, and the DM regression models parameterized in terms of delta and rho (see Guimaraes and Lindrooth, 2007).  Usage of this function is as follows:

dirmultreg(formula, data, disp, plant, poll, delta.vars, method)

Arguments:
formula – a symbolic description of the model to be fitted which has the form response ~ terms,  where response is the (numeric) response vector and terms is a series of terms which specifies a linear predictor for response, e.g., x1 + x2 indicates all the terms in first together with all the terms in second.
data – a data set containing the variables in the model as shown above.  Note these data must be an R object of class “data.frame”.
disp – dispersion structure to use:  “none” (GCL), “dconst” (δ constant), “dfunc” (γ function of covariates) and “rconst” (ρ constant).
plant – group identifier for plant species
poll – group identifier for pollinator species
delta.vars – list of predictor variables specified in formula to be used to model dispersion as a function of pollinator specific covariates, i.e., z1.  Default is NULL.
method – the method to be used for the optimization procedure: “Nelder- Mead”, “BFGS”, “L-BFGS”, “CG”, and “SANN”.  Default is “Nelder-Mead”.

Consider the data set above which consists of three linkage rules (x1 – x3), plant species relative abundance (x4) and pollinator species abundance (z1).  If we choose to run a model assuming no dispersion, then the command would be:

dirmultreg(formula = Y~x1+x2+x3+x4, data = filename, disp = “none”, plant = plant, poll = poll, delta.vars = NULL, method = “BFGS”)

Note that z1 was not used because we are specifying no dispersion, hence the dispersion parameter cannot be modeled as a function of covariates.  Also, the results of the simulation study suggested that method “BFGS” worked best for the no dispersion and intra-correlation models while “Nelder-Mead” worked best for the constant and function of covariate models.

Alternatively, if we chose to run a model assuming dispersion as a function of covariates, then the command would be:
 
dirmultreg(formula = Y~x1+x2+x3+x4+z1, data = filename, disp = “dfunc”, plant = plant, poll = poll, delta.vars = “z1”, method = “Nelder-Mead”)

In this example, we have added z1 to the formula and delta.vars arguments and specified “Nelder-Mead” for the optimization procedure.  

