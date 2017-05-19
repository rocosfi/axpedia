# Container manipulation
## Update an element with conPoke

  static void conPokeExample(Args _arg)
  {
      container c1 = ["item1", "item2", "item3"];
      container c2;
      int i;

      void conPrint(container c)
      {
          for (i = 1 ; i <= conLen(c) ; i++)
          {
              print conPeek(c, i);
          }
      }
      ;
      conPrint(c1);
      c2 = conPoke(c1, 2, "PokedItem");
      print "";
      conPrint(c2);
      pause;
  }
