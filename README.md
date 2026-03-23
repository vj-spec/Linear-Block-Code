# Linear-Block-Code
# Aim
Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 
# Tools required
* VS Code
# Program
```
import numpy as np

col = int(input("Enter the Parity bits: "))
row = int(input("Enter the Message bits: "))

pb = [list(map(int, input(f"Enter row {i+1} values (space-separated): ").split())) for i in range(row)]
p_mat = np.array(pb, dtype=int)
g_mat = np.hstack((p_mat, np.eye(row, dtype=int)))

n, k = g_mat.T.shape

m = np.array([[( i >> (k-j-1)) & 1 for j in range(k)] for i in range(2**k)])
c = np.mod(m @ g_mat, 2)
hw = c.sum(axis=1)
d_min = hw[1:].min()

hp = np.hstack((np.eye(n-k, dtype=int), p_mat[:, :3].T))
ht = hp.T

print(f'\nGenerator Matrix:\n{g_mat}')
print(f'\n{"Message":<15}{"Codeword":<15}{"H.Weight"}')
for i in range(len(m)):
    print(f'{" ".join(map(str,m[i])):<15}{" ".join(map(str,c[i])):<15}{hw[i]}')
print(f'\nMin Hamming Distance: {d_min}')
print(f'\nParity Check Matrix:\n{hp}')
print(f'\nParity Check Matrix Transpose:\n{ht}')

rc = np.array(list(map(int, input("\nEnter received codeword: ").split())))
syndrome = np.mod(rc @ ht, 2)
print(f'\nSyndrome: {" ".join(map(str, syndrome))}')

err_pos = next((i for i in range(n) if np.array_equal(syndrome, ht[i])), None)
err = np.eye(n, dtype=int)[err_pos] if err_pos is not None else np.zeros(n, dtype=int)
print(f'Error Position: {" ".join(map(str, err))}')
print(f'Corrected Codeword: {" ".join(map(str, (err + rc) % 2))}')
```
# Output Waveform

<img width="866" height="812" alt="Screenshot 2026-03-13 134502" src="https://github.com/user-attachments/assets/7d478e4a-c429-40a8-94db-603ed9fce029" />
<img width="623" height="490" alt="Screenshot 2026-03-13 135201" src="https://github.com/user-attachments/assets/6da6fe3f-d282-465e-8b1c-9955cd51114d" />

# Results
Thus linear block code operation for the given input is successfully verified.
