# MyPractise

import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics2D;
import java.awt.geom.Rectangle2D;
import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;

import org.jfree.chart.ChartFactory;
import org.jfree.chart.JFreeChart;
import org.jfree.chart.labels.StandardCategoryItemLabelGenerator;
import org.jfree.chart.plot.CategoryMarker;
import org.jfree.chart.plot.CategoryPlot;
import org.jfree.chart.plot.PlotOrientation;
import org.jfree.chart.renderer.category.BarRenderer;
import org.jfree.chart.renderer.category.CategoryItemRenderer;
import org.jfree.chart.renderer.category.LineAndShapeRenderer;
import org.jfree.chart.renderer.category.StandardBarPainter;
import org.jfree.data.category.DefaultCategoryDataset;
import org.jfree.ui.LengthAdjustmentType;
import org.jfree.ui.RectangleAnchor;
import org.jfree.ui.TextAnchor;

import com.itextpdf.awt.DefaultFontMapper;
import com.itextpdf.text.Document;
import com.itextpdf.text.Image;
import com.itextpdf.text.PageSize;
import com.itextpdf.text.pdf.PdfContentByte;
import com.itextpdf.text.pdf.PdfTemplate;
import com.itextpdf.text.pdf.PdfWriter;

public class MyStringClass {

	public static void main(String args[]) {

		DefaultCategoryDataset my_bar_chart_dataset = new DefaultCategoryDataset();
		my_bar_chart_dataset.addValue(34, "Q1", "Rome");
		my_bar_chart_dataset.addValue(45, "Q1", "Cairo");
		my_bar_chart_dataset.addValue(22, "Q2", "Rome");
		my_bar_chart_dataset.addValue(12, "Q2", "Cairo");
		my_bar_chart_dataset.addValue(56, "Q3", "Rome");
		my_bar_chart_dataset.addValue(98, "Q3", "Cairo");
		my_bar_chart_dataset.addValue(2, "Q4", "Rome");
		my_bar_chart_dataset.addValue(15, "Q4", "Cairo");
		my_bar_chart_dataset.addValue(34, "Q1", "sydney");
		my_bar_chart_dataset.addValue(45, "Q1", "india");
		my_bar_chart_dataset.addValue(22, "Q2", "sydney");
		my_bar_chart_dataset.addValue(12, "Q2", "india");
		my_bar_chart_dataset.addValue(56, "Q3", "sydney");
		my_bar_chart_dataset.addValue(98, "Q3", "india");
		my_bar_chart_dataset.addValue(2, "Q4", "sydney");
		my_bar_chart_dataset.addValue(15, "Q4", "india");
		my_bar_chart_dataset.addValue(34, "Q1", "Audtria");
		my_bar_chart_dataset.addValue(45, "Q1", "bhutan");
		my_bar_chart_dataset.addValue(22, "Q2", "Audtria");
		my_bar_chart_dataset.addValue(12, "Q2", "bhutan");
		my_bar_chart_dataset.addValue(56, "Q3", "Audtria");
		my_bar_chart_dataset.addValue(98, "Q3", "bhutan");
		my_bar_chart_dataset.addValue(2, "Q4", "Audtria");
		my_bar_chart_dataset.addValue(15, "Q4", "bhutan");

		System.out.println(my_bar_chart_dataset.getColumnCount());

		JFreeChart jFreeChart = ChartFactory.createBarChart("LaptopUsers BarChart", // title
				"Year", // categoryAxisLabel
				"LaptopUsers", // valueAxisLabel
				my_bar_chart_dataset, // dataset
				PlotOrientation.VERTICAL, // orientation
				true, true, false); // legend, tooltips and urls

		jFreeChart.getTitle().setPaint(Color.RED);
		jFreeChart.getPlot().setBackgroundPaint(Color.WHITE);

		Font font3 = new Font("Dialog", Font.PLAIN, 8);

		CategoryPlot plot = jFreeChart.getCategoryPlot();
		plot.setOutlineVisible(false);
		// plot.setDomainGridlinePaint(Color.black);
		plot.setRangeGridlinePaint(Color.LIGHT_GRAY);
		plot.getDomainAxis().setTickLabelFont(font3);
		plot.getRangeAxis().setTickLabelFont(font3);
		jFreeChart.getTitle().setFont(font3);
		plot.getRangeAxis().setLabelFont(font3);

		CategoryItemRenderer lineRenderer = new LineAndShapeRenderer();
		plot.setDataset(0, my_bar_chart_dataset);
		plot.setRenderer(0, lineRenderer);

		BarRenderer baRenderer = new BarRenderer();
		baRenderer.setBaseItemLabelGenerator( new StandardCategoryItemLabelGenerator());
		baRenderer.setBaseItemLabelsVisible(true);
		baRenderer.setShadowVisible(false);
		baRenderer.setBarPainter(new StandardBarPainter());
		plot.setDataset(1, my_bar_chart_dataset);

		CategoryMarker marker = new CategoryMarker("Rome");
		marker.setLabel("Band Y");
		marker.setPaint(Color.LIGHT_GRAY);
		marker.setOutlinePaint(Color.red);
		marker.setAlpha(0.9f);
		marker.setLabelAnchor(RectangleAnchor.BOTTOM);
		marker.setLabelTextAnchor(TextAnchor.TOP_CENTER);
		marker.setLabelOffsetType(LengthAdjustmentType.CONTRACT);
		
		plot.addDomainMarker(marker);
		plot.setRenderer(1, baRenderer);
		String pdfFilePath = "C:/Users/zk2y5oh/Desktop/iText1.pdf";
		OutputStream fos;
		try {
			fos = new FileOutputStream(new File(pdfFilePath));

			Document document = new Document(PageSize.A4.rotate());
			PdfWriter writer = PdfWriter.getInstance(document, fos);

			document.open();

			PdfContentByte pdfContentByte = writer.getDirectContent();
			Image image = Image.getInstance("C:/Users/zk2y5oh/Documents/My Received Files/footer_72.png");
			image.scaleToFit(840, 50);
			image.setAbsolutePosition(0f, 0f);
			pdfContentByte.addImage(image);
			int width = 300; // width of BarChart
			int height = 200; // height of BarChart
			PdfTemplate pdfTemplate = pdfContentByte.createTemplate(width, height);

			// create graphics
			Graphics2D graphics2d = pdfTemplate.createGraphics(300, 200, new DefaultFontMapper());

			// create rectangle
			Rectangle2D rectangle2d = new Rectangle2D.Double(0, 0, width, height);

			jFreeChart.draw(graphics2d, rectangle2d);

			graphics2d.dispose();
			pdfContentByte.addTemplate(pdfTemplate, 40, 300); // 0, 0 will draw BAR chart on bottom left of page

			document.close();
			System.out.println("PDF created in >> " + pdfFilePath);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	}

}
