# stajhaftasi1
using System;
using System.Drawing;
using System.Windows.Forms;

namespace StajProjesi
{
    public partial class Form1 : Form
    {
        const int rows = 20;
        const int cols = 20;
        const int cellSize = 25;
        

        Node[,] grid = new Node[rows, cols];

        public Form1()
        {
            InitializeComponent();

            
            for (int r = 0; r < rows; r++)
            {
                for (int c = 0; c < cols; c++)
                {
                    grid[r, c] = new Node(r, c);
                }
            }
        }
        private void btnStart_Click(object sender, EventArgs e)
        {
            
        }

        private void panelGrid_Load(object sender, EventArgs e)
        {
            
        }
        private void panelGrid_Paint(object sender, PaintEventArgs e)
        {
            Graphics g = e.Graphics;

            for (int r = 0; r < rows; r++)
            {
                for (int c = 0; c < cols; c++)
                {
                    Rectangle rect = new Rectangle(
                        c * cellSize,
                        r * cellSize,
                        cellSize,
                        cellSize);

                    Brush brush = Brushes.White;

                    if (grid[r, c].IsWall)
                        brush = Brushes.Black;
                    else if (grid[r, c].IsStart)
                        brush = Brushes.Green;
                    else if (grid[r, c].IsGoal)
                        brush = Brushes.Red;

                    g.FillRectangle(brush, rect);
                    g.DrawRectangle(Pens.Gray, rect);
                }
            }
        }

        private void panelGrid_MouseClick(object sender, MouseEventArgs e)
        {
            int col = e.X / cellSize;
            int row = e.Y / cellSize;

            if (row < 0 || row >= rows || col < 0 || col >= cols)
                return;

            Node clickedNode = grid[row, col];

            if (e.Button == MouseButtons.Left)
            {
                clickedNode.IsWall = !clickedNode.IsWall;
                clickedNode.IsStart = false;
                clickedNode.IsGoal = false;
            }
            else if (e.Button == MouseButtons.Right)
            {
                for (int r = 0; r < rows; r++)
                    for (int c = 0; c < cols; c++)
                        grid[r, c].IsGoal = false;

                clickedNode.IsGoal = true;
                clickedNode.IsWall = false;
                clickedNode.IsStart = false;
            }
            else if (e.Button == MouseButtons.Middle)
            {
                for (int r = 0; r < rows; r++)
                    for (int c = 0; c < cols; c++)
                        grid[r, c].IsStart = false;

                clickedNode.IsStart = true;
                clickedNode.IsWall = false;
                clickedNode.IsGoal = false;
            }

            panelGrid.Invalidate();

        }
    }
}
