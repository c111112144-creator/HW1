----------------------------------------------------------------------------------
-- Testbench for tc
----------------------------------------------------------------------------------
library IEEE;
use IEEE.std_logic_1164.all;
use IEEE.numeric_std.all;

entity tb_tc is
end tb_tc;

architecture Behavioral of tb_tc is

    -- 宣告 DUT 的 I/O
    signal clk       : std_logic := '0';
    signal rst       : std_logic := '0';
    signal count1_o  : std_logic_vector(7 downto 0);
    signal count2_o  : std_logic_vector(7 downto 0);

    -- 時鐘週期 (20 ns -> 50 MHz)
    constant clk_period : time := 20 ns;

begin

    -- DUT 實例化
    uut: entity work.tc
        port map (
            clk      => clk,
            rst      => rst,
            count1_o => count1_o,
            count2_o => count2_o
        );

    -- 產生時鐘
    clk_process : process
    begin
        clk <= '0';
        wait for clk_period/2;
        clk <= '1';
        wait for clk_period/2;
    end process;

    -- 測試序列
    stim_proc: process
    begin
        -- 一開始 reset 有效 (低態有效)
        rst <= '0';
        wait for 20 ns;

        -- 解除 reset
        rst <= '1';
        wait for 40 ns;

        -- 再次 reset
        rst <= '0';
        wait for 20 ns;
        rst <= '1';

        -- 模擬再跑一段時間
        wait for 40 ns;

        -- 停止模擬
        wait;
    end process;

end Behavioral;
