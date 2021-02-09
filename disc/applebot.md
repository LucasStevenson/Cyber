# AppleBot 2.0

> Points: 150

> Category: Misc

## Description
Welcome to AppleBot 2.0! C Edition! We fixed that nasty bug from last time, and migrated to a cleaner interface. Enjoy the apples!

## Attachments
[Download C code](https://fdownl.ga/569F7B3808)

``nc imaginary.ml 10042``

## Solution
```c
int main(void) {
  int choice, apples=5;
  long donation=0;
  setvbuf(stdout,NULL,2,0);
  setvbuf(stdin,NULL,2,0);
  printf("Welcome to AppleBot 2.0!\n");
  for (int i=0;i<50;i++) {
    printf("You currently have %d apples.\n", apples);
    printf("Choices:\n");
    printf("\t1. Work\n");
    printf("\t2. Donate apples to the AppleBot developers\n");
    printf("\t3. Buy flag(1000000000 apples)\n");
    printf("\t4. Exit\n");
    printf("> ");
    scanf("%d", &choice);
    ...
    else if (choice == 2) {
      printf("How many apples would you like to donate to the developers? ");
      scanf("%ld", &donation);
      if (donation > 0) {
        printf("Thanks for your donation!\n");
        apples -= donation;
      }
    ...
```
Amount of apples of stored as an **int**

We can perform an [integer overflow](https://www.sciencedirect.com/topics/computer-science/integer-overflow) attack by donating just the right amount of apples to make it wraparound. Once we have enough, we buy the flag.

### Flag
> ictf{s0_m4ny4ppl3s..._1s_4ppl3bot_4_sc4m???}
