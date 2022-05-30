class Main {
  public static void main(String[] args) throws Exception {
   double[] tempo = new double[5];
    double somaTempo = 0;
    for (int i=0; i<5; i++) {
    double time = calculapi(); 
    System.out.printf("Main: Tchau! Executou em: %f segundos\n", time);
    tempo[i] = time;
    somaTempo = somaTempo + tempo[i];
      }
    System.out.println("media do tempo de execucao: "+(somaTempo/5));
    double D = 0;
    double Dp = 0;
    for (int i=0;i<5;i++) {
        D = D + Math.pow((tempo[i]-(somaTempo/5)), 2);
        Dp = Dp + Math.pow(D/5, 0.5);
    } 
    System.out.println("Desvio padrao: "+Dp);
  }

    public static double calculapi() throws Exception {
    long startTime = System.currentTimeMillis();
    
    int numThreads = 4;
    Thread[] threads = new Thread[numThreads];
    int quantidade = 1000000/numThreads; 
     double[] somas = new double[numThreads];
    for (int i = 0; i < numThreads; i++) {
            int inicio = quantidade*i;
            int fim = quantidade *(i+1);
            System.out.printf("Main: criando thread: %02d\n", i);
            threads[i] = new CalculaPi(i, inicio, fim, somas);
            //threads[i]    .setDaemon(true); // evita que o programa principal espere pelas threads.
            threads[i].start();  // inicia thread
        }
    esperaThreads(threads);

    double pi = 0;
     for (int i = 0; i < numThreads; i++) {
       pi = pi + somas[i];
       }
    System.out.println(pi);
    long finishTime = System.currentTimeMillis();
        return (double)(finishTime - startTime)/(double)1000;
        
  }
  

  public static class CalculaPi extends Thread {
      int tid;
      int inicio;
      int fim;
      double[] somas;
     public CalculaPi(int tid, int inicio, int fim, double[] somas) {
            this.tid = tid;
            this.inicio = inicio;
            this.fim = fim;
            this.somas = somas;
        }
    public void run() {
      System.out.println(tid);  
      double soma = 0;
      for (int i = inicio; i < fim; i++) {
        double p1 = 4 * Math.pow(-1, i);
        double p2 = (2*i)+1.0;
        soma = soma + (p1/p2);
      }
       somas[tid] = soma;
    } 
  }
   public static void esperaThreads(Thread[] threads) throws InterruptedException {
        for (Thread t : threads) {
            t.join();
        } 
  }
}
