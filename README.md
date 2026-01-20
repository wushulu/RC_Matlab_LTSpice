# RC_Matlab_LTSpice
对比数字滤波和模拟滤波的关系
## RC IN LTspice
  设置RC电源：150V值的正弦、频率1Khz 比例是150V/V 使RC采样输入点的电压保证在1V
  <img width="946" height="693" alt="image" src="https://github.com/user-attachments/assets/131a766e-ac03-4920-be8f-54cd946f2615" />
  
  注意：R2和R3等效电阻等于(R2*R3)/(R2+R3) = 2.98K欧姆
  
  设置仿真：

  
  <img width="763" height="529" alt="image" src="https://github.com/user-attachments/assets/837245bb-fdbb-4e52-8bee-9a77554bd88e" />

  仿真-3bB 截止频率约为691Hz

  <img width="1586" height="975" alt="image" src="https://github.com/user-attachments/assets/dc369cad-9f63-4eca-9f7f-944eacd6dd99" />

## Matlab 
### 模拟到数字
- RC输出公式： $Vin(t)=RC\dfrac{dVout}{dt}+Vout$ 
  - 时间T采样换成x(n) y(n) :$x(n)=RC\dfrac{y(n)-y(n-1)}{T}+y(n)$
  - 得到如下公式:$y(n)=a*x(n)+(1-a)y(n-1)$
  - 计算a: $a=\dfrac{T}{RC+T}$ 
- 设置RC :R = 470+2980 c =68nf 采样时间T = 1e-5 所以a = 1e-5/(((2980+470) *68 *1e-9)+1e-5) = 0.040883
   

  <img width="907" height="630" alt="image" src="https://github.com/user-attachments/assets/14bb0e7e-7c84-4009-a060-d54bbbd87e18" />

  右键信号线-分别设置滤波器的输入和输出为开环输入和开环输出点

  <img width="807" height="368" alt="image" src="https://github.com/user-attachments/assets/617488ce-6f98-4a07-b8d8-d1976523ebc7" />

  APP-模型线性优化器 运行伯德图得出结果如下：截止频率 662Hz 与LTspice 691Hz 接近 存在误差可能是Matlb采样频率不够只有10Khz 
  
  <img width="1428" height="1035" alt="image" src="https://github.com/user-attachments/assets/7267ce01-1cd5-44bb-97e2-d1d5641477b4" />

  验证猜想：采样时间设置成1e-6即100K 并计算a 
  
  <img width="1203" height="761" alt="image" src="https://github.com/user-attachments/assets/a08e172c-50bf-4e96-90d5-4725cd08ebb9" />

  运行伯德图：截止频率约678Hz更接近LTspice
  
  <img width="1193" height="813" alt="image" src="https://github.com/user-attachments/assets/7d88ed7c-a7b8-489c-9bcf-76499833f800" />

## RC 差分信号并增加运放
  RC增加运放放，滤波器通过RC之后会再经过运放 放大倍数会限输入信号的带宽。
  运放带宽 M/Gain 就是能通过最大频率的信号。
  差分信号会存在零点和极点，可以通过计算出极点 即fc = 1/(2*pi*(R1+R2)*C) 与实际LTspice 仿真不一致。
  
  <img width="1844" height="759" alt="image" src="https://github.com/user-attachments/assets/059030e5-d3a6-491c-82a0-f575370787d7" />

  根据图中可算出fc= 795K

  实际运行仿真如下：约1.1MHz 确实越到高频阶段波形也会被'拉'起来
  
<img width="834" height="649" alt="image" src="https://github.com/user-attachments/assets/ef946fbd-7b34-4f22-a18d-304e93a1148f" />


  
