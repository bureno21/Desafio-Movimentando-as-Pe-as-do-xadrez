using System;

namespace DesafioMovimentandoAsPecasDoXadrez
{
    class Program
    {
        static void Main(string[] args)
        {
            char[,] tabuleiro = new char[8, 8];
            InicializarTabuleiro(tabuleiro);

            while (true)
            {
                Console.Clear();
                ExibirTabuleiro(tabuleiro);
                Console.WriteLine("\nDigite o movimento (ex: e2 e4) ou 'sair' para encerrar:");
                string entrada = Console.ReadLine();

                if (entrada.ToLower() == "sair")
                    break;

                string[] partes = entrada.Split(' ');
                if (partes.Length != 2)
                {
                    Console.WriteLine("Entrada inválida! Pressione Enter para tentar novamente...");
                    Console.ReadLine();
                    continue;
                }

                string origem = partes[0];
                string destino = partes[1];

                if (!MoverPeca(tabuleiro, origem, destino))
                {
                    Console.WriteLine("Movimento inválido! Pressione Enter para tentar novamente...");
                    Console.ReadLine();
                }
            }
        }

        static void InicializarTabuleiro(char[,] t)
        {
            // Peças brancas
            t[0, 0] = 'T'; t[0, 1] = 'C'; t[0, 2] = 'B'; t[0, 3] = 'D'; t[0, 4] = 'R'; t[0, 5] = 'B'; t[0, 6] = 'C'; t[0, 7] = 'T';
            for (int i = 0; i < 8; i++) t[1, i] = 'P';

            // Espaços vazios
            for (int i = 2; i < 6; i++)
                for (int j = 0; j < 8; j++)
                    t[i, j] = '.';

            // Peças pretas
            for (int i = 0; i < 8; i++) t[6, i] = 'p';
            t[7, 0] = 't'; t[7, 1] = 'c'; t[7, 2] = 'b'; t[7, 3] = 'd'; t[7, 4] = 'r'; t[7, 5] = 'b'; t[7, 6] = 'c'; t[7, 7] = 't';
        }

        static void ExibirTabuleiro(char[,] t)
        {
            Console.WriteLine("  a b c d e f g h");
            for (int i = 7; i >= 0; i--)
            {
                Console.Write((i + 1) + " ");
                for (int j = 0; j < 8; j++)
                    Console.Write(t[i, j] + " ");
                Console.WriteLine();
            }
        }

        static bool MoverPeca(char[,] t, string origem, string destino)
        {
            int origemCol = origem[0] - 'a';
            int origemLin = origem[1] - '1';
            int destinoCol = destino[0] - 'a';
            int destinoLin = destino[1] - '1';

            if (!PosicaoValida(origemLin, origemCol) || !PosicaoValida(destinoLin, destinoCol))
                return false;

            char peca = t[origemLin, origemCol];
            if (peca == '.')
                return false;

            // Movimento simples, sem validações reais de regra
            t[destinoLin, destinoCol] = peca;
            t[origemLin, origemCol] = '.';
            return true;
        }

        static bool PosicaoValida(int l, int c)
        {
            return l >= 0 && l < 8 && c >= 0 && c < 8;
        }
    }
}
