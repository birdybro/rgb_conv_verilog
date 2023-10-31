# rgb_conv_verilog
different rgb colorspace conversion techniques from rgb_xxx to rgb_888 for mister

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
