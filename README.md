# rgb_conv_verilog
different rgb colorspace conversion techniques from rgb_xxx to rgb_888 for fpga video

## rgb_222 to rgb_888 conversion with verilog/systemverilog using concatenation

this one's the easiest, there are no issues with scaling the colors up since it divides evenly (i think):

```sv
  logic [1:0] r_2, g_2, b_2;
  logic [7:0] r_8, g_8, b_8;

  always_comb begin
    r_8 = {4{r_2[1]}, 4{r_2[0]}};
    g_8 = {4{g_2[1]}, 4{g_2[0]}};
    b_8 = {4{b_2[1]}, 4{b_2[0]}};
  end

  assign VGA_R = r_8;
  assign VGA_G = g_8;
  assign VGA_B = b_8;
```

## rgb_333 to rgb_888 conversion with verilog/systemverilog using a pre-generated lookup table

add rgb333_to_rgb888.* files to your project

for includes in qip:

```ini
set_global_assignment -name QIP_FILE rtl/rgb333_to_rgb888.qip
```

and then add code to instantiate the module:

```sv
  logic [2:0] r_3, g_3, b_3;
  logic [7:0] r_8, g_8, b_8;

  rgb333_to_rgb888 rgb333_to_rgb888
  (
    .address({r_3, g_3, b_3}),
    .clock(clk_vid), // video clock, not pixel clock!
    .q({r_8, g_8, b_8)
  );

  assign VGA_R = r_8;
  assign VGA_G = g_8;
  assign VGA_B = b_8;  
```

## rgb_555 to rgb_888 conversion with verilog/systemverilog using linear interpolation

```sv
  logic [4:0] r_5, g_5, b_5;
  logic [7:0] r_8, g_8, b_8;

  always_comb begin
    r_8 = (r_5 << 3) + (r_5 >> 2);
    g_8 = (g_5 << 3) + (g_5 >> 2);
    b_8 = (b_5 << 3) + (b_5 >> 2);
  end

  assign VGA_R = r_8;
  assign VGA_G = g_8;
  assign VGA_B = b_8;
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
