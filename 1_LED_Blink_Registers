// Allumer une LED avec la datasheet et attendre 1000ms avant de l'éteindre

void config_initial(void);
void attendre(unsigned int temps);

int main(void)
{
  unsigned int *portD = (unsigned int*) 0x40020C00;

  config_initial();

  /* Infinite loop */
  while (1)
  {
    *(portD + (0x14 / 4)) ^= (0x01 << 12);
    attendre(1000);
  }
}
void config_initial(void)
{
  // Pour activer la LED vert (LED4), on trouve sur la doc de la carte qu'il est connecté sur le pin PD12,
  // il faut activer le port D des GPIOs
  // activation du port D
  volatile unsigned int* portRCC; portRCC = (volatile unsigned int*) 0x40023830;
  do
    *portRCC |= (0x01 << 3);
  while (!(*portRCC & (0x01 << 3)));  //Delay after an RCC peripheral clock enabling

  volatile unsigned int *portD = (volatile unsigned int*) 0x40020C00;
  
  // on veut configurer le pin 12 en sortie  c-a-d les bits 25 et 24 du registre MODER du port D à 01
  unsigned int moder = *portD;
  moder &= (unsigned int)~(0x3 << 12*2);  // on met les bits 25 et 24 du registre MODER du port D à 00
  moder |= (unsigned int) (0x1 << 12*2);  // on met le bit 24 du registre MODER du port D à 1
  *portD = moder;

  // on veut configurer le pin 12 en connexion push-pull c-a-d le bit 12 du registre OTYPER du port D à 0
  *(portD + 1) &= ~(0x1 << 12);   // on met le bit 12 du registre OTYPER du port D à 1 (décalage 1 => 4 octets)

}
void attendre(unsigned int temps)
{
  for (int i = 0; i < temps*4000; i++);  // 16MHz/4 ins par for = 4 000 000 loops/sec=> 1ms = 4000 loops
}
