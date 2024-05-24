# Model Parameter Data

## Header
Parent has to be  named CLAUDEModelParameter
```xml
<CLAUDEModelParameter>
    <!-- model parameters here -->
</CLAUDEModelParameter>
```

## Model
### FlagLinearModel
Whether it is a linear model or no. Value is "T" if yes
```python
self.flagLinearModel = nodeValue == "T"
```

### MinimizationMethod
* Method: string
* Tolerance: float
* MaxIter: int
* FTol: float
* GTol: float
* Eps: float
* Disp: boolean, T for true

### ModeInitialPrecisionMatrixMax
integer. mode for initial precision matrix such that likelihood becomes maximum.

### OffsetInitialPrecisionMatrixMax
integer. offset for initial precision matrix such that likelihood becomes maximum.

## Descriptor
### NumberDescriptor
* The number of descriptor
```python
self.nDescriptor = integer value of NumberDescriptor
self.dimensionDescriptor = self.nDescriptor + 1
```

### DegreePolynomialDescriptor
If linear model, self.degreePolynomialDescriptor is a matrix with shape of (self.dimensionDescriptor,self.nDescriptor) of 0 , except on [i,i-1] for i in range (1,self.dimensionDescriptor).
Else, read several nodes of DegreePolynomialDescriptor, convert each nodes as array, then combine the array to be matrix. Example:
```xml
<DegreePolynomialDescriptor>
    0 1 1 0
</DegreePolynomialDescriptor>
<DegreePolynomialDescriptor>
    0 1 0 1
</DegreePolynomialDescriptor>
<DegreePolynomialDescriptor>
    0 1 0 1
</DegreePolynomialDescriptor>
```
Will be converted to [[0,1,1,0],[0,1,0,1],[0,1,0,1]]. Note that length of each row is self.dimensionDescriptor

## Target
### NumberTarget
The number of target
```python
self.nTarget = integer value of NumberTarget
self.dimensionTarget = self.nTarget + 1
self.dimension = self.dimensionDescriptor + self.dimensionTarget
```

### DegreePolynomialTarget
Same as DegreePolynomialDescriptor, but no relation with Linear Model

### ActiveVariable
Whether variable active or not? confirm. A lot of code is skipped if it is not active. self.activeVariable is matrix of shape [dimensionTarget, dimension], initially set as False. For example,
```xml
<ActiveDescriptor index=IDX1>idx2</ActiveDescriptor>
```
will lead to 
```python
self.activeVariable[idx1,idx2] = True
```
However, if 
```python
idx2 > dimensionDescriptor
```
We will have
```
self.activeVariable[idx2 - self.dimensionDescriptor,idx1 + self.dimensionDescriptor] = True #flipped from above case?
```
self.activeVariable[i,i + self.dimensionDescriptor] must be true

### InactiveVariable
Same as active variable, but set as false
