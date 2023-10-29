----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date:    19:07:39 10/29/2023 
-- Design Name: 
-- Module Name:    FSMTest - Behavioral 
-- Project Name: 
-- Target Devices: 
-- Tool versions: 
-- Description: 
--
-- Dependencies: 
--
-- Revision: 
-- Revision 0.01 - File Created
-- Additional Comments: 
--
----------------------------------------------------------------------------------
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;
use IEEE.numeric_std.all;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx primitives in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity FSMTest is
    Port ( Clock : in  STD_LOGIC;
           PB1 : in  STD_LOGIC;
           PB2 : in  STD_LOGIC;
           Q : out  STD_LOGIC_VECTOR (3 downto 0));
end FSMTest;

architecture Behavioral of FSMTest is
 type state_type is (S0, S1, S2);
 signal state, next_state : state_type;
 signal r_reg : UNSIGNED(3 downto 0);
 signal r_next : UNSIGNED(3 downto 0);
begin

 -- state register
 process (Clock,PB2)
 begin
  if (PB2 = '1') then state <=S0;
  elsif (Clock'event and Clock = '1') then
  state <= next_state;
  r_reg <= r_next;
  end if;
  end process;

-- state logic
process (state, PB1, PB2)
begin

case state is 
	when S0 => 
		if PB1 ='1' then 
			r_next <=r_reg +1;
			next_state <= S1;
		else 
			r_next <= (others => '0');
			next_state <= S0;
		end if;
	when S1 => 
		if PB1 ='1' then 
			r_next <= r_reg +1;
			next_state <= S1;
	else
		r_next <= r_reg;
		
	end if;
	if r_reg = "1111" then 
	next_state <= S2;
	else 
	next_state <= S1;
	end if;
	when S2 => 
	r_next <= r_reg;
	next_state <= S2;
end case;
end process;

--output
process (state, r_reg)
begin

case state is when S0 | S1 | S2 => Q <=
STD_LOGIC_VECTOR(r_reg);
end case;
end process;
end Behavioral;
