#heatmap
#Load packages
library(gplots)
library(RColorBrewer)
#cargar los datos y transformarlos a una matriz 
datos <- jaccard_kegg2
rnames <- datos[,1] #asignar los nombres de la columna 1 a "rnames"
jaccard_KEGG <- data.matrix(datos[,c(2,3)]) #transforma las columnas 2 y 3 a matrix
rownames(matriz_datos) <- rnames 

col <- colorRampPalette(brewer.pal(9, "RdPu"))

# El siguiente código limita el color del rango, respectivamente
#quantile.range <- quantile(matriz_datos, prob = seq(0, 1, length = 11), type = 5)
#palette.breaks <- seq(quantile.range["25%"], quantile.range["50%"], 
      #                quantile.range["75%"], quantile.range["100%"], 0.01)

col_breaks = c(seq(0,0.03,length.out = 50), 
           seq(0.04,0.05,length= 50), 
          seq(0.06,0.08,length.out = 50),
         seq(0.09, 0.15, length.out = 50), 
         seq(0.16, 0.3, length.out = 50))             

png(file = "jaccar16.png", width = 2800, height = 2800)
heatmap.2(matriz_datos, 
          #Rowv = TRUE,
          #Colv=if(symm)"Rowv" else TRUE,
          #distfun = dist,
          #notecol="black",
          #hclustfun = hclust,
          dendrogram = "row",
          #reorderfun = function(d, w) reorder(d, w),
          col = col,
          #scale = "row",
          key = TRUE,
          keysize= 1,
          #key.xlab = NULL,
          main = "Jaccard index for Apoptosis and Autophagy \nversus KEGG pathways",
          #xlab = NULL,
          #ylab = NULL,
          #key.title = TRUE,
          trace="none", 
          breaks = col_breaks,
          margins = c(8,13),
          cexRow = 0.5,
          cexCol = 2,
          srtCol = 40,
          colsep = TRUE, 
          density.info = "density")
dev.off()          
# para que se vea mejor el heatmap, queremos quitar todas las vias que tengan jaccard cero con respecto a apoptosis y autofagia
# queremos una nueva tabla que no tenga todos esos valores:
hist(jaccard_KEGG, breaks = 100, col = "Red")


