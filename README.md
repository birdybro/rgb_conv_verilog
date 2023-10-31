# rgb_conv_verilog
different rgb colorspace conversion techniques from rgb_xxx to rgb_888 for fpga video

## rgb_555 to rgb_888 conversion with verilog/systemverilog using linear interpolation

```sv
  logic [4:0] r_555, g_555, b_555;
  logic [7:0] r_888, g_888, b_888;

  always_comb begin
    r_888 = (r_555 << 3) + (r_555 >> 2);
    g_888 = (g_555 << 3) + (g_555 >> 2);
    b_888 = (b_555 << 3) + (b_555 >> 2);
  end

  assign VGA_R = r_888;
  assign VGA_G = g_888;
  assign VGA_B = b_888;
```

## rgb_565 to rgb_888 conversion with verilog/systemverilog using concatenation

```sv
  logic [4:0] r_5, b_5;
  logic [5:0] g_6
  logic [7:0] r_8, g_8, b_8;

  always_comb begin
    r_8 = {r_5, r_5[2:0]};
    g_8 = {g_6, g_6[1:0]};
    b_8 = {b_5, b_5[2:0]};
  end

  assign VGA_R = r_8;
  assign VGA_G = g_8;
  assign VGA_B = b_8;
```

## rgb_565 to rgb_888 conversion with verilog/systemverilog using shifts (less accurate)

```sv
  logic [4:0] r_5, b_5;
  logic [5:0] g_6
  logic [7:0] r_8, g_8, b_8;

  always_comb begin
    r_8 = r_5 << 3;
    g_8 = g_6 << 2;
    b_8 = b_5 << 3;
  end

  assign VGA_R = r_8;
  assign VGA_G = g_8;
  assign VGA_B = b_8;
```
