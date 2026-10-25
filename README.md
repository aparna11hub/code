Exp 2: To simulate Friis Transmission formula and Estimate the path loss. 

clc; clear;

Pt=input("Pt (W): "); Gt=input("Gt: "); Gr=input("Gr: ");
f=input("Frequency (Hz): "); d=input("Distance (m): ");

W=3e8/f;
Pr=Pt*Gt*Gr*W^2/(4*%pi*d)^2;

disp(msprintf("Pr(%d m) = %e W",d,Pr));
disp(msprintf("Pr(dB) = %.2f dB",10*log10(Pr)));
disp(msprintf("Pr(dBm) = %.2f dBm",10*log10(Pr/0.001)));

d=1:30;
subplot(2,1,1);
plot(d,91.52+20*log10(d));
title("Path Loss for 900 MHz");
xlabel("Distance (km)"); ylabel("Path Loss (dB)");

subplot(2,1,2);
plot(d,97.54+20*log10(d));
title("Path Loss for 1800 MHz");
xlabel("Distance (km)"); ylabel("Path Loss (dB)");

Exp 3: To observe the effect of N on C/I ratio and comment on the voice quality.

clc; clear;
n=4; i0=6; T=[];
for i=1:5
    for j=0:5
        N=i^2+i*j+j^2;
        Q=sqrt(3*N);
        T=[T;i j N Q 10*log10(Q*n/i0)];
    end
end
[ a,k ]=gsort(T(:,3),"g","i"); T=T(k,:);
i=1;
while i<size(T,1)
    if T(i,3)==T(i+1,3) then T(i,:)=[];
    else i=i+1; end
end
mprintf("%-3s %-3s %-7s %-9s %-10s\n","i","j","N","Q","C/I(dB)");
for r=1:size(T,1)
    mprintf("%-3d %-3d %-7.2f %-9.3f %-10.3f\n",T(r,1),T(r,2),T(r,3),T(r,4),T(r,5));
end

Exp 4: To observe the effect of velocity and direction of arrival of a vehicle on Doppler frequency.

clc; clear;

fc=input("Carrier frequency: ");
Vm=input("Mobile speed (m/s): ");
theta=input("Angle (degrees): ");

fd=Vm*fc*cosd(theta)/3e8;
fr=fc+fd;

printf("\nDoppler Shift = %.2f Hz\n",fd);
printf("Received Frequency = %.2f Hz\n",fr);

Exp 6: To generate PN sequence using SCILAB.

clc; clear;
a=zeros(15,4);
a(1,:)=input("Initial state: ");

for i=2:15
    a(i,1)=bitxor(a(i-1,3),a(i-1,4));
    a(i,2:4)=a(i-1,1:3);
end

disp("The state table is");
disp(a);
y=a(:,4)';
disp("PN sequence is");
disp(y);
b=sum(y); c=15-b;
disp("The number of ones:"); disp(b);
disp("The number of zeroes:"); disp(c);

Exp 7: To implement Code Division Multiple Access (CDMA)

clc; clear;

A=input("Enter Data Bit for Sender A (0 or 1): ");
B=input("Enter Data Bit for Sender B (0 or 1): ");

Ad=2*A-1; Bd=2*B-1;
cA=2*[0 0 1 1 0 0 1 1]-1;
cB=2*[0 1 1 0 0 1 1 0]-1;

As=Ad*cA; Bs=Bd*cB; Cs=As+Bs;
sA=sum(Cs.*cA); sB=sum(Cs.*cB);
rA=double(sA>0); rB=double(sB>0);

disp("--- TRANSMITTER ---");
disp("Bipolar Data Ad:",Ad);
disp("Bipolar Data Bd:",Bd);
disp("Spread Data As:",As);
disp("Spread Data Bs:",Bs);
disp("--- CHANNEL ---");
disp("Combined Signal Cs:",Cs);
disp("--- RECEIVER ---");
disp("Receiver A Sum:",sA);
disp("Recovered Data A:",rA);
disp("Receiver B Sum:",sB);
disp("Recovered Data B:",rB);

Exp 8: To write program for convolutional encoder using SCILAB.

clc; a=input('Enter input:'); a(7:14)=0; m=zeros(1,9);
for j=1:14
    m=[a(j),m(1:8)];
    v1(j)=modulo(sum(m([1 2 3 4 6 8 9])),2);
    v2(j)=modulo(sum(m([1 3 4 5 9])),2);
end
b=[v1;v2];
disp('Output at v1:'); disp(v1);
disp('Output at v2:'); disp(v2);
disp('Final output:'); disp(b(:)');

Exp 9: To generate Walsh code using SCILAB.

clc; clear;
n=input("Enter order: "); H=1;
while size(H,1)<n
    H=[H H;H -H];
end
disp("Hadamard Matrix:"); disp(H);
c=sum(H(:,2:n)<>H(:,1:n-1),"c");
[s,k]=gsort(c,"g","i"); W=H(k,:);
disp("Walsh Codes:"); disp(W);

Exp 10:  A) Write a program to plot SNR versus channel capacity for different MIMO systems

clc; clear;
x=2:2:20;
plot(x,log(1+10.^(x/10))/log(2));
xlabel('SNR'); ylabel('Channel Capacity');
title('SNR Vs Channel Capacity FOR different MIMO systems');

colors=['r','g','m'];
for k=2:4
    y=mean(k*log(1+10.^(x'/10).*abs(rand(1,10000,'normal')))/log(2),'c');
    plot(x,y,colors(k-1));
end

