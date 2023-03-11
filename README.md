# Checking if Four Numbers Represent a Rectangle Using Quantum Computing
# This code checks if four given numbers represent the sides of a rectangle. The approach used in this code is to convert each number to binary and check if the binary representation of the numbers satisfies the condition of a rectangle.
# Requirements
# Python 3.x
# Qiskit
#Usage
#Clone the repository:
#git clone https://github.com/18001-spec/for-qosf-task-evaluation.git
#Modify the input values num1, num2, num3, and num4 to any four numbers of your choice.
#Run the code:
#Copy code
#python rectangle.py
#The output will be either 1 or 0. A value of 1 indicates that the four numbers represent a rectangle, while a value of 0 indicates that they do not.
#How it Works
#The code takes in four numbers as input, num1, num2, num3, and num4, and converts them to their binary representation using the format() #function. The binary strings are then padded with zeros on the left to ensure that they are all of equal length.
#Two quantum circuits are created, one to compare the binary representation of num1 and num2, and another to compare the binary #representation of num3 and num4. The circuits are created using the Qiskit framework.
#The quantum circuit for num1 and num2 is constructed as follows:
#A QuantumRegister object qreg is created with a number of qubits equal to the length of the binary string.
#A ClassicalRegister object creg is created with a number of bits equal to the length of the binary string.
#A QuantumCircuit object qc1 is created using qreg and creg.
#X gates are applied to each qubit in qreg for every bit in the binary string that is a 1 for both num1 and num2.
#CX gates are applied to pairs of qubits in qreg to compare the two binary strings.
#The qubits in qreg are measured and the resulting bitstring is stored in creg.
#The circuit is executed using the execute() function and the results are retrieved using the result() and get_counts() functions.
#The quantum circuit for num3 and num4 is constructed in the same way as the quantum circuit for num1 and num2.
#If the binary representation of num1 and num2 satisfies the condition of a rectangle and the binary representation of num3 and num4 satisfies #the same condition, the output will be 1. Otherwise, the output will be 0.
#Limitations
#The approach used in this code is only applicable for small numbers. For larger numbers, the quantum circuits required to compare the binary #strings would become too large to be executed on current quantum hardware.
#The qasm_simulator backend is used to execute the quantum circuits. This backend simulates the behavior of a quantum computer using a #classical computer. While this allows the code to be executed on any computer, it also means that the code does not take advantage of the unique features of quantum hardware.
#References
#Qiskit Documentation
# FOR-QOSF-TASKS-EVALUATION

    from qiskit import QuantumCircuit, Aer, execute
    def is_rectangle(num1, num2, num3, num4):
    # convert numbers to binary
    bin1 = format(num1, 'b')
    bin2 = format(num4, 'b')
    bin3 = format(num2, 'b')
    bin4 = format(num3, 'b')

    # make sure the binary strings have the same length
    max_len = max(len(bin1), len(bin2), len(bin3), len(bin4))
    bin1 = bin1.zfill(max_len)
    bin2 = bin2.zfill(max_len)
    bin3 = bin3.zfill(max_len)
    bin4 = bin4.zfill(max_len)

    # create quantum circuit for num1 and num2
    n = len(bin1)
    qreg = QuantumRegister(n, name='qreg')
    creg = ClassicalRegister(n, name='creg')
    qc1 = QuantumCircuit(qreg, creg)

    # apply X gates for 1 bits in binary strings
    for i in range(n):
        if bin1[i] == '1':
            qc1.x(qreg[i])
        if bin2[i] == '1':
            qc1.x(qreg[i])

    # apply CX gates to compare the binary strings
    for i in range((n-1)//2):
        qc1.cx(qreg[i], qreg[(n-1)-i])

    # measure the qubits
    qc1.measure(qreg, creg)

    # execute the circuit for num1 and num2 and print the results
    backend = Aer.get_backend('qasm_simulator')
    job = execute(qc1, backend, shots=1)
    result = job.result()
    counts = result.get_counts(qc1)

    equal_14 = '0'*n in counts

    # create quantum circuit for num3 and num4
    qreg = QuantumRegister(n, name='qreg')
    creg = ClassicalRegister(n, name='creg')
    qc2 = QuantumCircuit(qreg, creg)

    # apply X gates for 1 bits in binary strings
    for i in range(n):
        if bin3[i] == '1':
            qc2.x(qreg[i])
        if bin4[i] == '1':
            qc2.x(qreg[i])

    # apply CX gates to compare the binary strings
    for i in range((n-1)//2):
        qc2.cx(qreg[i], qreg[(n-1)-i])

    # measure the qubits
    qc2.measure(qreg, creg)

    # execute the circuit for num3 and num4 and print the results
    backend = Aer.get_backend('qasm_simulator')
    job = execute(qc2, backend, shots=1)
    result = job.result()
    counts = result.get_counts(qc2)

    equal_23 = '0'*n in counts

    if equal_14 and equal_23:
        print("1")
    else:
        print("0")
is_rectangle(5555, 8, 8, 5555) #expected_output is 1
