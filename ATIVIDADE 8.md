				O que são OpenMP tasks?
Uma OpenMP task ou tarefa OpenMP é uma única linha de código ou um bloco
estruturado, o qual é escrito é escrito abaixo em uma lista de tarefas.

					Como utilizar?
omp_set_num_threads( 2 ); //define o número de threads que serão usadas
#pragma omp parallel default(none) //sem isso, a thread#0 irá fazer tudo
{
	#pragma omp task
	fprintf(txt, "%s\n", “A”);
	/*Salva o caractere 'A' com uma quebra de linha no arquivo indicado
	pelo ponteiro "txt" e coloca "fprintf" na lista de tarefas*/
	
	#pragma omp task
	fprintf(txt, "%s\n", “B”);
	/*O segundo caractere 'B' com uma quebra de linha também é salvo no
	mesmo arquivo e "fprintf" também é colocada na lista de tarefas*/
}

					Exemplos de utilização
O uso das OpenMP tasks é indicado para reduzir a complexidade de tarefas
que tem custo elevado para o hardware, como por exemplo, processar todos
os elementos de uma lista ligada ou em uma árvore.

				Diferenças para as OpenMP Sections
Tasks são colocadas em fila e executadas em quaquer momento até o ponto
de adiamento ou scheduling point.
O conceito de Sections no OpenMP divide o código que está entre as chaves
para as threads existentes. A principal diferença entre elas está no tempo
em que o código será executado e na divisão de tarefas feita entre as
threads no modo OpenMP task.

						Como utilizar?
O uso do OpenMP Section inicia com a diretiva "#omp parallel" seguida
pela palavra "sections", ficando #omp parallel sections
Cada bloco de código que será incluído em uma section deve ficar dentro
de um trecho que inicia da seguinte forma:

#pragma omp section
{
	//escreva seu código aqui
}

A diretiva #pragma omp section deve ser incluída dentro do bloco de chaves
de #omp parallel sections. Mais de um bloco de código pode ser incluído
dentro do bloco sections. Ex:

#pragma omp parallel sections //início do bloco que irá conter as sections
{
	#omp pragma section //início da primeira section
	{
		//escreva seu código aqui
	}
	#pragma omp section //início de segunda section
	{
		//escreva seu código aqui
	}
} 
Cada bloco de código dentro da diretiva sections é executado por uma
única thread. Existe uma thread para a primeira section e uma outra
thread para a segunda section. O conceito de OpenMP usa threads para
alcançar o paralelismo, que é a execução de tarefas diferentes e de
forma simultânea. Contudo, se houver mais blocos de código ou sections
do que threads, pode ocorrer de uma única thread ficar com mais tarefas
do que outras. Isso quebra o paralelismo, fazendo com que a divisão de
tarefas nem sempre seja feita de forma equilibrada.

					
Já para o uso de OpenMP tasks, a estrutura do código é praticamente a mesma,
alterando apenas a palavra sections para task.
Ex:

#pragma omp parallel
{
	#pragma omp task //primeira tarefa que será colocada em fila
	//escreva seu código aqui

	#pragma omp task //segunda tarefa que será colocada em fila
	//escreva seu código aqui
}

				Aplicações das OpenMP task e OpenMP Sections
Sistemas de memória compartilhada, automação de tarefas de custo
elevado, tanto em quesitos de tempo e de recursos utilizados.
