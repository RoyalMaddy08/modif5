# modif5

LTSpice code 

2a. 
* KNN Classifier Circuit (Corrected)
 * This circuit calculates squared Euclidean distances and currents
 * for 3 trained examples compared to 1 input example
 * Voltage sources for input example (1.0, 2.0, 3.0)
 V1 in1 0 1
 V2 in2 0 2
 V3 in3 0 3
 * Voltage sources for trained examples
 * Example 1: (1.5, 2.5, 3.5)
 V4 ex1_1 0 1.5
 V5 ex1_2 0 2.5
 V6 ex1_3 0 3.5
 * Example 2: (0.5, 1.5, 2.5)
 V7 ex2_1 0 0.5
 V8 ex2_2 0 1.5
V9 ex2_3 0 2.5
 * Example 3: (1.0, 2.0, 3.0)
 V10 ex3_1 0 1
 V11 ex3_2 0 2
 V12 ex3_3 0 3
 * Difference calculation
 E1 diff1_1 0 in1 ex1_1 1
 E2 diff1_2 0 in2 ex1_2 1
 E3 diff1_3 0 in3 ex1_3 1
 E4 diff2_1 0 in1 ex2_1 1
 E5 diff2_2 0 in2 ex2_2 1
 E6 diff2_3 0 in3 ex2_3 1
 E7 diff3_1 0 in1 ex3_1 1
 E8 diff3_2 0 in2 ex3_2 1
 E9 diff3_3 0 in3 ex3_3 1
 * Squaring the differences
 B1 sq1_1 0 V=V(diff1_1)*V(diff1_1)
 B2 sq1_2 0 V=V(diff1_2)*V(diff1_2)
 B3 sq1_3 0 V=V(diff1_3)*V(diff1_3)
 B4 sq2_1 0 V=V(diff2_1)*V(diff2_1)
 B5 sq2_2 0 V=V(diff2_2)*V(diff2_2)
 B6 sq2_3 0 V=V(diff2_3)*V(diff2_3)
 B7 sq3_1 0 V=V(diff3_1)*V(diff3_1)
 B8 sq3_2 0 V=V(diff3_2)*V(diff3_2)
 B9 sq3_3 0 V=V(diff3_3)*V(diff3_3)
 * Summing squared differences
 B10 dist1 0 V=V(sq1_1)+V(sq1_2)+V(sq1_3)
 B11 dist2 0 V=V(sq2_1)+V(sq2_2)+V(sq2_3)
 B12 dist3 0 V=V(sq3_1)+V(sq3_2)+V(sq3_3)
 * Calculating currents (I = G * V * distance)
 B13 curr1 0 I=0.01*1*V(dist1)
B14 curr2 0 I=0.01*1*V(dist2)
 B15 curr3 0 I=0.01*1*V(dist3)
 * Adding dummy resistors to ground to prevent floating nodes
 R1 curr1 0 1Meg
 R2 curr2 0 1Meg
 R3 curr3 0 1Meg
 * Analysis command
 .op
 * Measurement commands
 .meas op dist1_mag PARAM V(dist1)
 .meas op dist2_mag PARAM V(dist2)
 .meas op dist3_mag PARAM V(dist3)
 .meas op curr1_mag PARAM abs(I(R1))
 .meas op curr2_mag PARAM abs(I(R2))
 .meas op curr3_mag PARAM abs(I(R3))
 .backanno
.end




2b. 
* Squared Euclidean Distance and Current Calculator
 * This circuit calculates the squared Euclidean distance between two 3D
 vectors
 * and the resulting current based on RRAM parameters
 * Voltage sources for input example (1.0, 2.0, 3.0)
 V1 in1 0 1
 V2 in2 0 2
 V3 in3 0 3
 * Voltage sources for trained example (1.5, 2.5, 3.5)
 V4 tr1 0 1.5
 V5 tr2 0 2.5
 V6 tr3 0 3.5
 * Difference calculation
 E1 diff1 0 in1 tr1 1
 E2 diff2 0 in2 tr2 1
 E3 diff3 0 in3 tr3 1
* Squaring the differences
 B1 sq1 0 V=V(diff1)*V(diff1)
 B2 sq2 0 V=V(diff2)*V(diff2)
 B3 sq3 0 V=V(diff3)*V(diff3)
 * Summing squared differences to get squared Euclidean distance
 B4 distance 0 V=V(sq1)+V(sq2)+V(sq3)
 * Calculating current (I = G * V * distance)
 * G_u = 1e-3 Siemens, V_u = 1.0 Volts
 B5 current 0 I=1e-3*1*V(distance)
 * Adding dummy resistor to ground to prevent floating node
 R1 current 0 1Meg
 * Analysis command
 .op
 * Measurement commands
 .meas op squared_distance PARAM V(distance)
 .meas op total_current PARAM abs(I(R1))
 .backanno
 .end


2c.
 * Corrected Binary Squared Euclidean Distance and Current Calculator
 * This circuit calculates the binary squared Euclidean distance between
 two 5-bit vectors
 * and the resulting current based on RRAM parameters
 * Voltage sources for input example (1, 0, 1, 0, 1)
 V1 in1 0 1
 V2 in2 0 0
 V3 in3 0 1
 V4 in4 0 0
 V5 in5 0 1
 * Voltage sources for trained example (1, 1, 0, 0, 1)
 V6 tr1 0 1
 V7 tr2 0 1
 V8 tr3 0 0
 V9 tr4 0 0
 V10 tr5 0 1
 * XOR operation for each bit using behavioral voltage sources
 * XOR(a, b) = (a + b)-2 * (a * b)
 Bxor1 xor1 0 V=(V(in1) + V(tr1))-2 * (V(in1) * V(tr1))
 Bxor2 xor2 0 V=(V(in2) + V(tr2))-2 * (V(in2) * V(tr2))
 Bxor3 xor3 0 V=(V(in3) + V(tr3))-2 * (V(in3) * V(tr3))
 Bxor4 xor4 0 V=(V(in4) + V(tr4))-2 * (V(in4) * V(tr4))
 Bxor5 xor5 0 V=(V(in5) + V(tr5))-2 * (V(in5) * V(tr5))
 * Sum of XOR results (binary squared Euclidean distance)
 Bdist distance 0 V=V(xor1)+V(xor2)+V(xor3)+V(xor4)+V(xor5)
 * Calculating current (I = G * V * distance)
 * G_u = 1e-3 Siemens, V_u = 1.0 Volts
 Bcurrent 0 current I=1e-3*V(distance)
 * Adding dummy resistor to ground to prevent floating node
 R1 current 0 1Meg
 * Analysis command
 .op
 * Measurement commands
 .meas op binary_squared_distance PARAM V(distance)
 .meas op total_current PARAM I(Bcurrent)
 .backanno
 .end

2d.
 * RRAM-based Parallel Computing Architecture for kNN Classification
 * Define parameters
 .param G_u = 1m ; Unit conductance in Siemens (1 mS)
 .param V_u = 2.0 ; Unit read voltage in Volts (2 V)
 .param V_ETH = 0.005 ; Dynamic threshold voltage
 * Input attributes (voltage sources)
 V1 N1 0 4.0 ; Input attribute a1(x)
 V2 N2 0 6.0 ; Input attribute a2(x)
 V3 N3 0 8.0 ; Input attribute a3(x)
 * Stored attributes (voltage sources)
 V4 N4 0 3.0 ; Stored attribute a1(x_i)
 V5 N5 0 5.0 ; Stored attribute a2(x_i)
 V6 N6 0 7.0 ; Stored attribute a3(x_i)
 * RRAM cells (modeled as resistors)
 R1 N1 N4 {1/G_u} ; RRAM cell for attribute 1
 R2 N2 N5 {1/G_u} ; RRAM cell for attribute 2
 R3 N3 N6 {1/G_u} ; RRAM cell for attribute 3
 * Current summing using behavioral current sources
 B1 0 Nsum I=G_u * (V(N1)-V(N4))**2
 B2 0 Nsum I=G_u * (V(N2)-V(N5))**2
 B3 0 Nsum I=G_u * (V(N3)-V(N6))**2
 * Convert summed current to voltage
 Rsum Nsum 0 1k
 * Operational Amplifier (for square root operation)
 Eamp Nsqrt 0 VALUE={sqrt(V(Nsum))}
 * Comparator
 Ecomp Out 0 VALUE={if(V(Nsqrt)<V_ETH, 5, 0)}
 * Output resistor
 Rout Out 0 10k
 * Operating point analysis
 .OP
 .end


2e.
* Define parameters
 .param V_CTH = 2.0 ; Threshold voltage for class detection
 .param amplification = 1.0 ; Amplification factor for the operational
 amplifier
 * Example detector outputs (1 for high, 0 for low)
 * These can be adjusted based on the example detector outputs
 V1 N1 0 1.0 ; Example detector output 1
 V2 N2 0 1.0 ; Example detector output 2
 V3 N3 0 0.0 ; Example detector output 3
 V4 N4 0 1.0 ; Example detector output 4
 V5 N5 0 0.0 ; Example detector output 5
 * Summing the outputs of example detectors
 * Using an operational amplifier in summing mode
 Esum Nsum 0 VALUE={V(N1) + V(N2) + V(N3) + V(N4) + V(N5)}
 * Operational Amplifier (for amplifying summed voltage)
 Eamp Nout 0 VALUE={V(Nsum) * amplification}
 * Comparator to check threshold
 Ecomp Out 0 VALUE={if(V(Nout) > V_CTH, 5, 0)}
 * Output resistor
 Rout Out 0 10k
 * Operating point analysis
 .OP
 .end
