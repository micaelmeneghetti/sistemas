# sistemas

```VHDL

ENTITY controlblock IS
  PORT (
		  ck: IN BIT; -- clock
        start: IN BIT; 
        reset: IN BIT; 
		  
		  M0, M1, M2: OUT BIT_VECTOR (1 DOWNTO 0);
		  LX, LS, LH: OUT BIT;
		  P: OUT BIT;
		  H: OUT BIT); -- saida
		  
END controlblock;

ARCHITECTURE fsm OF controlblock IS 
  TYPE st IS (caso_f, caso_e, caso_d, caso_c, caso_b, caso_a); --novo tipo definido
  SIGNAL estado : st; -- sinal estado tipo "st"
  SIGNAL q : BIT_VECTOR (2 DOWNTO 0);
BEGIN
  PROCESS (ck, reset)
  BEGIN
    IF reset = '1' THEN 
      estado <= caso_a;  
    ELSIF (ck'EVENT and ck ='1') THEN 
      CASE estado IS
        WHEN caso_a =>                            
          IF start = '1' THEN estado <= caso_b;   
          ELSE estado <= caso_a;                  
          END IF;
        WHEN caso_b =>                       
          estado <= caso_c;
        WHEN caso_c => 
          estado <= caso_d;
        WHEN caso_d =>                          
          estado <= caso_e;
		  WHEN caso_e =>                          
          estado <= caso_f;
		  WHEN caso_f =>                          
          estado <= caso_a; 
      END CASE;
    END IF;
  END PROCESS;
  
 
 
 PROCESS (estado)
 BEGIN
  CASE estado IS
	WHEN caso_a =>
		LH <= '0';
		LX <= '1';
		LS <= '0';
		M2 <= "00";
		M1 <= "00";
		M0 <= "00";
	WHEN caso_b =>
		LH <= '1';
		M2 <= "00";
		M1 <= "01";
		H <= '1';
	WHEN caso_c =>
		M0 <= "01";
		M2 <= "01";
		M1 <= "00";
		H <= '1';
		LH <= '1';
	WHEN caso_d =>
		M0 <= "10";
		M2 <= "00";
		M1 <= "00";
		H <= '1';
		LS <= '1';
	WHEN caso_e =>
		M2 <= "11";
		M1 <= "10";
		H <= '0';
		LH <= '1';
	WHEN caso_f =>
		M2 <= "11";
		M0 <= "11";
		M1 <= "00";
		H <= '0';
		LS <= '1';
		P <= '1';
	END CASE;
 END PROCESS;
			
END fsm;

```
