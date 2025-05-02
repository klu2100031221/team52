# team52
import org.apache.poi.xwpf.usermodel.XWPFDocument;
import org.apache.poi.xwpf.usermodel.XWPFParagraph;
import org.apache.poi.xwpf.usermodel.XWPFRun;
import org.apache.commons.csv.CSVFormat;
import org.apache.commons.csv.CSVParser;
import org.apache.commons.csv.CSVRecord;
import org.testng.Assert;
import org.testng.annotations.Test;

import java.io.*;
import java.nio.charset.StandardCharsets;
import java.nio.file.Paths;

public class CSVToWordTest {

    // Method to get the latest downloaded CSV file (based on the latest modified timestamp)
    public static File getLatestCSVFile(String downloadDirPath) {
        File dir = new File(downloadDirPath);
        File[] csvFiles = dir.listFiles((d, name) -> name.toLowerCase().endsWith(".csv"));
        if (csvFiles == null || csvFiles.length == 0) {
            return null;
        }
        return csvFiles[0];  // Return the first file if found, can be customized
    }

    @Test
    public void testCSVToWord() {
        String downloadDirPath = "C:\\Users\\YourName\\Downloads"; // Adjust your download folder path
        String docxFilePath = "C:\\Users\\YourName\\Documents\\output.docx"; // Adjust the output DOCX file path

        // Get the latest CSV file from the downloads folder
        File csvFile = getLatestCSVFile(downloadDirPath);
        Assert.assertNotNull(csvFile, "No CSV file found in the download folder!");

        // Read the CSV and write to DOCX
        boolean fileCreated = false;
        try (
                Reader reader = new InputStreamReader(new FileInputStream(csvFile), StandardCharsets.UTF_8);
                CSVParser parser = new CSVParser(reader, CSVFormat.DEFAULT.withFirstRecordAsHeader());
                XWPFDocument document = new XWPFDocument();
                FileOutputStream out = new FileOutputStream(docxFilePath)
        ) {
            // Process CSV records and add to Word document
            for (CSVRecord record : parser) {
                XWPFParagraph paragraph = document.createParagraph();
                XWPFRun run = paragraph.createRun();
                StringBuilder line = new StringBuilder();

                // Append all CSV columns to the Word document
                for (String header : parser.getHeaderMap().keySet()) {
                    line.append(header).append(": ").append(record.get(header)).append(" | ");
                }

                run.setText(line.toString());
            }

            // Write document to output file
            document.write(out);
            fileCreated = true;
            System.out.println("Word document created: " + Paths.get(docxFilePath).toAbsolutePath());
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Assert that the Word document was successfully created
        Assert.assertTrue(fileCreated && new File(docxFilePath).exists(), "Word document was not created!");
    }
}
