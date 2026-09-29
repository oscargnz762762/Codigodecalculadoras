package calculadora_figuras;

import javax.swing.*;
import javax.swing.border.AbstractBorder;
import javax.swing.plaf.basic.BasicButtonUI;
import javax.swing.plaf.basic.BasicComboBoxUI;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class CalculadoraFigurasUI extends JFrame {
    private JPanel panelPrincipal;
    private JButton btnCalcular;
    private JButton btnLimpiar;
    private JComboBox listaFiguras;
    private JLabel lblLado;
    private JTextField txtLado;
    private JTextField txtBase;
    private JTextField txtAltura;
    private JTextField txtRadio;
    private JTextField txtBaseMayor;
    private JTextField txtBaseMenor;
    private JTextField txtDiagonalMayor;
    private JTextField txtDiagonalMenor;
    private JTextField txtNumLados;
    private JTextField txtApotema;
    private JLabel lblBase;
    private JLabel lblAltura;
    private JLabel lblRadio;
    private JLabel lblBaseMayor;
    private JLabel lblBaseMenor;
    private JLabel lblDiagonalMayor;
    private JLabel lblDiagonalMenor;
    private JLabel lblNumLados;
    private JLabel lblApotema;
    private JLabel lblArea;
    private JLabel lblPerimetro;
    private static final Color COLOR_BG = new Color(235, 238, 243);
    private static final Color COLOR_INPUT_BG = new Color(225, 229, 235);
    private static final Color COLOR_LIGHT_SHADOW = new Color(255, 255, 255, 220);
    private static final Color COLOR_DARK_SHADOW = new Color(190, 198, 210, 180);
    private static final Color COLOR_ACCENT_ORANGE = new Color(245, 158, 11);
    private static final Color COLOR_ACCENT_ORANGE_GRADIENT = new Color(234, 88, 12);
    private static final Color COLOR_TEXT_DARK = new Color(60, 72, 88);

    public CalculadoraFigurasUI() {
        setContentPane(panelPrincipal);
        setTitle("Calculadora de Figuras Geométricas");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        listaFiguras.setModel(new DefaultComboBoxModel<>(new String[]{
                "Cuadrado",
                "Rectángulo",
                "Triángulo",
                "Círculo",
                "Trapecio",
                "Rombo",
                "Polígono Regular",
                "Paralelogramo"
        }));
        aplicarEstiloNeumorfico();

        setMinimumSize(new Dimension(450, 580));
        setLocationRelativeTo(null);
        actualizarVisibilidadCampos();
        listaFiguras.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                actualizarVisibilidadCampos();
            }
        });

        btnCalcular.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                calcularResultados();
            }
        });

        btnLimpiar.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                limpiarCampos();
            }
        });
    }
    private void aplicarEstiloNeumorfico() {
        panelPrincipal.setBackground(COLOR_BG);
        JTextField[] camposTexto = {
                txtLado, txtBase, txtAltura, txtRadio, txtBaseMayor,
                txtBaseMenor, txtDiagonalMayor, txtDiagonalMenor, txtNumLados, txtApotema
        };
        for (JTextField txt : camposTexto) {
            if (txt != null) {
                txt.setOpaque(true);
                txt.setBackground(COLOR_INPUT_BG);
                txt.setFont(new Font("SansSerif", Font.BOLD, 14));
                txt.setForeground(COLOR_TEXT_DARK);
                txt.setCaretColor(COLOR_ACCENT_ORANGE);
                txt.setEditable(true);
                txt.setEnabled(true);
                txt.setBorder(new NeuSunkenBorder(10));
            }
        }
        JLabel[] etiquetas = {
                lblLado, lblBase, lblAltura, lblRadio, lblBaseMayor,
                lblBaseMenor, lblDiagonalMayor, lblDiagonalMenor, lblNumLados, lblApotema
        };
        for (JLabel lbl : etiquetas) {
            if (lbl != null) {
                lbl.setFont(new Font("SansSerif", Font.BOLD, 13));
                lbl.setForeground(COLOR_TEXT_DARK);
            }
        }
        if (lblArea != null) {
            lblArea.setFont(new Font("SansSerif", Font.BOLD, 16));
            lblArea.setForeground(COLOR_ACCENT_ORANGE);
        }
        if (lblPerimetro != null) {
            lblPerimetro.setFont(new Font("SansSerif", Font.BOLD, 16));
            lblPerimetro.setForeground(COLOR_TEXT_DARK);
        }
        if (listaFiguras != null) {
            listaFiguras.setFont(new Font("SansSerif", Font.BOLD, 14));
            listaFiguras.setForeground(COLOR_TEXT_DARK);
            listaFiguras.setBackground(COLOR_BG);
            listaFiguras.setFocusable(false);
            listaFiguras.setUI(new BasicComboBoxUI() {
                @Override
                protected JButton createArrowButton() {
                    JButton button = new JButton("▼");
                    button.setFont(new Font("SansSerif", Font.BOLD, 10));
                    button.setForeground(COLOR_ACCENT_ORANGE);
                    button.setContentAreaFilled(false);
                    button.setBorderPainted(false);
                    button.setFocusPainted(false);
                    return button;
                }
            });
        }
        if (btnCalcular != null) estilizarBotonCalcular(btnCalcular);
        if (btnLimpiar != null) estilizarBotonLimpiar(btnLimpiar);
    }

    private void estilizarBotonCalcular(JButton btn) {
        btn.setFocusPainted(false);
        btn.setContentAreaFilled(false);
        btn.setBorderPainted(false);
        btn.setFont(new Font("SansSerif", Font.BOLD, 15));
        btn.setForeground(Color.WHITE);
        btn.setCursor(new Cursor(Cursor.HAND_CURSOR));
        btn.setUI(new BasicButtonUI() {
            @Override
            public void paint(Graphics g, JComponent c) {
                Graphics2D g2 = (Graphics2D) g.create();
                g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                int w = c.getWidth();
                int h = c.getHeight();
                int radius = 18;

                boolean pressed = ((AbstractButton) c).getModel().isPressed();
                if (!pressed) {
                    g2.setColor(new Color(245, 158, 11, 60));
                    g2.fillRoundRect(2, 2, w - 4, h - 4, radius, radius);

                    GradientPaint gp = new GradientPaint(0, 0, COLOR_ACCENT_ORANGE, 0, h, COLOR_ACCENT_ORANGE_GRADIENT);
                    g2.setPaint(gp);
                    g2.fillRoundRect(0, 0, w - 2, h - 2, radius, radius);
                } else {
                    GradientPaint gp = new GradientPaint(0, 0, COLOR_ACCENT_ORANGE_GRADIENT, 0, h, COLOR_ACCENT_ORANGE);
                    g2.setPaint(gp);
                    g2.fillRoundRect(1, 1, w - 2, h - 2, radius, radius);
                }
                g2.dispose();
                super.paint(g, c);
            }
        });
    }

    private void estilizarBotonLimpiar(JButton btn) {
        btn.setFocusPainted(false);
        btn.setContentAreaFilled(false);
        btn.setBorderPainted(false);
        btn.setFont(new Font("SansSerif", Font.BOLD, 15));
        btn.setForeground(COLOR_TEXT_DARK);
        btn.setCursor(new Cursor(Cursor.HAND_CURSOR));
        btn.setUI(new BasicButtonUI() {
            @Override
            public void paint(Graphics g, JComponent c) {
                Graphics2D g2 = (Graphics2D) g.create();
                g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
                int w = c.getWidth();
                int h = c.getHeight();
                int radius = 18;
                int offset = 3;

                boolean pressed = ((AbstractButton) c).getModel().isPressed();
                if (!pressed) {
                    g2.setColor(COLOR_LIGHT_SHADOW);
                    g2.fillRoundRect(0, 0, w - offset, h - offset, radius, radius);

                    g2.setColor(COLOR_DARK_SHADOW);
                    g2.fillRoundRect(offset, offset, w - offset, h - offset, radius, radius);

                    g2.setColor(COLOR_BG);
                    g2.fillRoundRect(offset / 2, offset / 2, w - offset, h - offset, radius, radius);
                } else {
                    g2.setColor(COLOR_DARK_SHADOW);
                    g2.fillRoundRect(2, 2, w - 4, h - 4, radius, radius);

                    g2.setColor(COLOR_BG);
                    g2.fillRoundRect(3, 3, w - 6, h - 6, radius, radius);
                }
                g2.dispose();
                super.paint(g, c);
            }
        });
    }
    private void actualizarVisibilidadCampos() {
        if (lblLado != null) lblLado.setVisible(false); if (txtLado != null) txtLado.setVisible(false);
        if (lblBase != null) lblBase.setVisible(false); if (txtBase != null) txtBase.setVisible(false);
        if (lblAltura != null) lblAltura.setVisible(false); if (txtAltura != null) txtAltura.setVisible(false);
        if (lblRadio != null) lblRadio.setVisible(false); if (txtRadio != null) txtRadio.setVisible(false);
        if (lblBaseMayor != null) lblBaseMayor.setVisible(false); if (txtBaseMayor != null) txtBaseMayor.setVisible(false);
        if (lblBaseMenor != null) lblBaseMenor.setVisible(false); if (txtBaseMenor != null) txtBaseMenor.setVisible(false);
        if (lblDiagonalMayor != null) lblDiagonalMayor.setVisible(false); if (txtDiagonalMayor != null) txtDiagonalMayor.setVisible(false);
        if (lblDiagonalMenor != null) lblDiagonalMenor.setVisible(false); if (txtDiagonalMenor != null) txtDiagonalMenor.setVisible(false);
        if (lblNumLados != null) lblNumLados.setVisible(false); if (txtNumLados != null) txtNumLados.setVisible(false);
        if (lblApotema != null) lblApotema.setVisible(false); if (txtApotema != null) txtApotema.setVisible(false);

        String figura = (String) listaFiguras.getSelectedItem();
        if (figura == null) return;

        switch (figura) {
            case "Cuadrado":
                if (lblLado != null) lblLado.setVisible(true);
                if (txtLado != null) txtLado.setVisible(true);
                break;
            case "Rectángulo":
            case "Paralelogramo":
                if (lblBase != null) lblBase.setVisible(true); if (txtBase != null) txtBase.setVisible(true);
                if (lblAltura != null) lblAltura.setVisible(true); if (txtAltura != null) txtAltura.setVisible(true);
                break;
            case "Triángulo":
                if (lblBase != null) lblBase.setVisible(true); if (txtBase != null) txtBase.setVisible(true);
                if (lblAltura != null) lblAltura.setVisible(true); if (txtAltura != null) txtAltura.setVisible(true);
                if (lblLado != null) lblLado.setVisible(true); if (txtLado != null) txtLado.setVisible(true);
                break;
            case "Círculo":
                if (lblRadio != null) lblRadio.setVisible(true); if (txtRadio != null) txtRadio.setVisible(true);
                break;
            case "Trapecio":
                if (lblBaseMayor != null) lblBaseMayor.setVisible(true); if (txtBaseMayor != null) txtBaseMayor.setVisible(true);
                if (lblBaseMenor != null) lblBaseMenor.setVisible(true); if (txtBaseMenor != null) txtBaseMenor.setVisible(true);
                if (lblAltura != null) lblAltura.setVisible(true); if (txtAltura != null) txtAltura.setVisible(true);
                if (lblLado != null) lblLado.setVisible(true); if (txtLado != null) txtLado.setVisible(true);
                break;
            case "Rombo":
                if (lblDiagonalMayor != null) lblDiagonalMayor.setVisible(true); if (txtDiagonalMayor != null) txtDiagonalMayor.setVisible(true);
                if (lblDiagonalMenor != null) lblDiagonalMenor.setVisible(true); if (txtDiagonalMenor != null) txtDiagonalMenor.setVisible(true);
                if (lblLado != null) lblLado.setVisible(true); if (txtLado != null) txtLado.setVisible(true);
                break;
            case "Polígono Regular":
                if (lblNumLados != null) lblNumLados.setVisible(true); if (txtNumLados != null) txtNumLados.setVisible(true);
                if (lblLado != null) lblLado.setVisible(true); if (txtLado != null) txtLado.setVisible(true);
                if (lblApotema != null) lblApotema.setVisible(true); if (txtApotema != null) txtApotema.setVisible(true);
                break;
        }

        if (panelPrincipal != null) {
            panelPrincipal.revalidate();
            panelPrincipal.repaint();
        }

        pack();
        if (getWidth() < 450 || getHeight() < 580) {
            setSize(450, 580);
        }
    }

    private void calcularResultados() {
        String figura = (String) listaFiguras.getSelectedItem();
        if (figura == null) return;

        try {
            double area = 0;
            double perimetro = 0;

            switch (figura) {
                case "Cuadrado":
                    double lado = Double.parseDouble(txtLado.getText().trim());
                    area = lado * lado;
                    perimetro = 4 * lado;
                    break;
                case "Rectángulo":
                case "Paralelogramo":
                    double base = Double.parseDouble(txtBase.getText().trim());
                    double altura = Double.parseDouble(txtAltura.getText().trim());
                    area = base * altura;
                    perimetro = 2 * (base + altura);
                    break;
                case "Triángulo":
                    double b = Double.parseDouble(txtBase.getText().trim());
                    double h = Double.parseDouble(txtAltura.getText().trim());
                    double lTriangulo = Double.parseDouble(txtLado.getText().trim());
                    area = (b * h) / 2;
                    perimetro = 3 * lTriangulo;
                    break;
                case "Círculo":
                    double radio = Double.parseDouble(txtRadio.getText().trim());
                    area = Math.PI * Math.pow(radio, 2);
                    perimetro = 2 * Math.PI * radio;
                    break;
                case "Trapecio":
                    double bMayor = Double.parseDouble(txtBaseMayor.getText().trim());
                    double bMenor = Double.parseDouble(txtBaseMenor.getText().trim());
                    double hTrap = Double.parseDouble(txtAltura.getText().trim());
                    double lTrap = Double.parseDouble(txtLado.getText().trim());
                    area = ((bMayor + bMenor) * hTrap) / 2;
                    perimetro = bMayor + bMenor + (2 * lTrap);
                    break;
                case "Rombo":
                    double dMayor = Double.parseDouble(txtDiagonalMayor.getText().trim());
                    double dMenor = Double.parseDouble(txtDiagonalMenor.getText().trim());
                    double lRombo = Double.parseDouble(txtLado.getText().trim());
                    area = (dMayor * dMenor) / 2;
                    perimetro = 4 * lRombo;
                    break;
                case "Polígono Regular":
                    int nLados = Integer.parseInt(txtNumLados.getText().trim());
                    double lLado = Double.parseDouble(txtLado.getText().trim());
                    double apotema = Double.parseDouble(txtApotema.getText().trim());
                    perimetro = nLados * lLado;
                    area = (perimetro * apotema) / 2;
                    break;
            }

            lblArea.setText(String.format("Área: %.2f", area));
            lblPerimetro.setText(String.format("Perímetro: %.2f", perimetro));

        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(this, "Por favor ingrese números válidos en los campos visibles.", "Error de entrada", JOptionPane.ERROR_MESSAGE);
        }
    }

    private void limpiarCampos() {
        JTextField[] camposTexto = {
                txtLado, txtBase, txtAltura, txtRadio, txtBaseMayor,
                txtBaseMenor, txtDiagonalMayor, txtDiagonalMenor, txtNumLados, txtApotema
        };
        for (JTextField txt : camposTexto) {
            if (txt != null) txt.setText("");
        }
        if (lblArea != null) lblArea.setText("Área: 0.00");
        if (lblPerimetro != null) lblPerimetro.setText("Perímetro: 0.00");
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            CalculadoraFigurasUI frame = new CalculadoraFigurasUI();
            frame.setVisible(true);
        });
    }
    static class NeuSunkenBorder extends AbstractBorder {
        private final int radius;

        public NeuSunkenBorder(int radius) {
            this.radius = radius;
        }

        @Override
        public void paintBorder(Component c, Graphics g, int x, int y, int width, int height) {
            Graphics2D g2 = (Graphics2D) g.create();
            g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);

            // Sombra oscura superior/izquierda (efecto hundido)
            g2.setColor(new Color(175, 183, 195));
            g2.drawRoundRect(x, y, width - 1, height - 1, radius, radius);
            g2.drawRoundRect(x + 1, y + 1, width - 3, height - 3, radius - 2, radius - 2);

            // Brillo blanco inferior/derecho
            g2.setColor(COLOR_LIGHT_SHADOW);
            g2.drawRoundRect(x + 2, y + 2, width - 4, height - 4, radius - 2, radius - 2);

            g2.dispose();
        }

        @Override
        public Insets getBorderInsets(Component c) {
            return new Insets(8, 12, 8, 12);
        }

        @Override
        public Insets getBorderInsets(Component c, Insets insets) {
            insets.left = 12;
            insets.top = 8;
            insets.right = 12;
            insets.bottom = 8;
            return insets;
        }
    }
}
