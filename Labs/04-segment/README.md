# Lab 4: Seven-segment display decoder

## 1:
### Figure connection of 7-segment displays on Nexys A7 board.
![obr2](Images/7seg.png)
Connection of 7-segment displays on Nexys A7 board

Zapojení se společnou anodou (zavedením ‚0’ na segment jej rozsvítí)

![obr3](Images/Connection_of_7-segment_displays_on_Nexys_A7_board.png)

### Decoder truth table for common anode 7-segment display.

| **Hex** | **Inputs** | **A** | **B** | **C** | **D** | **E** | **F** | **G** |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0000 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |
| 1 | 0001 | 1 | 0 | 0 | 1 | 1 | 1 | 1 |
| 2 | 0010 | 0 | 0 | 1 | 0 | 0 | 1 | 0 |
| 3 | 0011 | 0 | 0 | 0 | 0 | 1 | 1 | 0 |
| 4 | 0100 | 1 | 0 | 0 | 1 | 1 | 0 | 0 |
| 5 | 0101 | 0 | 1 | 0 | 0 | 1 | 0 | 0 |
| 6 | 0110 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 7 | 0111 | 0 | 0 | 0 | 1 | 1 | 1 | 1 |
| 8 | 1000 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 9 | 1001 | 0 | 0 | 0 | 0 | 1 | 0 | 0 |
| A | 1010 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |
| b | 1011 | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| C | 1100 | 0 | 1 | 1 | 0 | 0 | 0 | 1 |
| d | 1101 | 1 | 0 | 0 | 0 | 0 | 1 | 0 |
| E | 1110 | 0 | 1 | 1 | 0 | 0 | 0 | 0 |
| F | 1111 | 0 | 1 | 1 | 1 | 0 | 0 | 0 |

## 2:
### Listing of VHDL architecture from source file hex_7seg.vhd with syntax highlighting.
```VHDL
architecture Behavioral of hex_7seg is
begin
    p_7seg_decoder : process(hex_i)
    begin
        case hex_i is
            when "0000" =>
                seg_o <= "0000001";     -- 0
            when "0001" =>
                seg_o <= "1001111";     -- 1
    
            -- WRITE YOUR CODE HERE
            when "0010" =>
                seg_o <= "0010010";     -- 2
            when "0011" =>
                seg_o <= "0000110";     -- 3
            when "0100" =>
                seg_o <= "1001100";     -- 4
            when "0101" =>
                seg_o <= "0100100";     -- 5
            when "0110" =>
                seg_o <= "0100000";     -- 6
            when "0111" =>
                seg_o <= "0001111";     -- 7
            when "1000" =>
                seg_o <= "0000000";     -- 8
            when "1001" =>
                seg_o <= "0000100";     -- 9
            when "1010" =>
                seg_o <= "0001000";     -- A
            when "1011" =>
                seg_o <= "1100001";     -- B
            when "1100" =>
                seg_o <= "0110001";     -- C
            when "1101" =>
                seg_o <= "1000010";     -- D
            -- WRITE YOUR CODE HERE
            
            when "1110" =>
                seg_o <= "0110000";     -- E
            when others =>
                seg_o <= "0111000";     -- F
        end case;
    end process p_7seg_decoder;

end Behavioral;
```

### Listing of VHDL stimulus process from testbench file tb_hex_7seg.vhd with syntax highlighting and asserts.
```VHDL
p_stimulus : process
    begin
     -- Report a note at the begining of stimulus process
        report "Stimulus process started" severity note;

        s_hex <= "0000"; wait for 100 ns; --0
        
        s_hex <= "0001"; wait for 100 ns; --1
        
        s_hex <= "0010"; wait for 100 ns; --2
        
        s_hex <= "0011"; wait for 100 ns; --3
        
        s_hex <= "0100"; wait for 100 ns; --4
        
        s_hex <= "0101"; wait for 100 ns; --5
        
        s_hex <= "0110"; wait for 100 ns; --6
        
        s_hex <= "0111"; wait for 100 ns; --7
        
        s_hex <= "1000"; wait for 100 ns; --8
        
        s_hex <= "1001"; wait for 100 ns; --9
        
        s_hex <= "1010"; wait for 100 ns; --A
        
        s_hex <= "1011"; wait for 100 ns; --B
        
        s_hex <= "1100"; wait for 100 ns; --C
        
        s_hex <= "1101"; wait for 100 ns; --D
        
        s_hex <= "1110"; wait for 100 ns; --E
        
        s_hex <= "1111"; wait for 100 ns; --F
          
         -- Report a note at the end of stimulus process
        report "Stimulus process finished" severity note;
        wait;     
           
    end process p_stimulus;
```

### Screenshot with simulated time waveforms; always display all inputs and outputs.
![obr1](Images/Simulation.png)

### Listing of VHDL code from source file top.vhd with 7-segment module instantiation.
```VHDL
entity top is
    Port ( SW  : in  STD_LOGIC_VECTOR (4 - 1 downto 0);
           LED : out STD_LOGIC_VECTOR (8 - 1 downto 0);
           CA  : out STD_LOGIC;
           CB  : out STD_LOGIC;
           CC  : out STD_LOGIC;
           CD  : out STD_LOGIC;
           CE  : out STD_LOGIC;
           CF  : out STD_LOGIC;
           CG  : out STD_LOGIC;
           AN  : out STD_LOGIC_VECTOR (8 - 1 downto 0)
          );
end top;

architecture Behavioral of top is
begin
---------------------------------------------------------------------------
      -- Instance (copy) of hex_7seg entity
    hex2seg : entity work.hex_7seg
        port map(
            hex_i    => SW,
            seg_o(6) => CA,

            -- WRITE YOUR CODE HERE
            seg_o(5) => CB,
            seg_o(4) => CC,
            seg_o(3) => CD,
            seg_o(2) => CE,
            seg_o(1) => CF,
            seg_o(0) => CG
        );

    -- Connect one common anode to 3.3V
    AN <= b"1111_0111";

    -- Display input value
    LED(3 downto 0) <= SW;
```

## 3:
### Truth table and listing of VHDL code for LEDs(7:4) with syntax highlighting.
| **Hex** | **Inputs** | **LED4** | **LED5** | **LED6** | **LED7** |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0000 | 1 | 0 | 0 | 0 |
| 1 | 0001 | 0 | 0 | 1 | 1 |
| 2 | 0010 | 0 | 0 | 0 | 1 |
| 3 | 0011 | 0 | 0 | 1 | 0 |
| 4 | 0100 | 0 | 0 | 0 | 1 |
| 5 | 0101 | 0 | 0 | 1 | 0 |
| 6 | 0110 | 0 | 0 | 0 | 0 |
| 7 | 0111 | 0 | 0 | 1 | 0 |
| 8 | 1000 | 0 | 0 | 0 | 1 |
| 9 | 1001 | 0 | 1 | 1 | 0 |
| A | 1010 | 0 | 1 | 0 | 0 |
| b | 1011 | 0 | 1 | 1 | 0 |
| C | 1100 | 0 | 1 | 0 | 0 |
| d | 1101 | 0 | 1 | 1 | 0 |
| E | 1110 | 0 | 1 | 0 | 0 |
| F | 1111 | 0 | 1 | 1 | 1 |


