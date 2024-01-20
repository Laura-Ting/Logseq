- #+BEGIN_QUERY
  {:title "Not uploaded images"
    :query [:find (pull ?b [*])
          :where
          [?b :block/page ?p]
          [?p :block/name ?page_name]
          (not [(clojure.string/includes? ?page_name "created by logseq-plugin-image-uploader")])
          [?b :block/content ?content]
          (not [(clojure.string/includes? ?content "{:title \"Not uploaded images\"")])
          [(clojure.string/includes? ?content "](../assets")]
          [(clojure.string/includes? ?content "![")]
          (or [(clojure.string/includes? ?content ".png)")]
              [(clojure.string/includes? ?content ".jpg)")]
              [(clojure.string/includes? ?content ".jpeg)")]
              [(clojure.string/includes? ?content ".gif)")]
              [(clojure.string/includes? ?content ".tiff)")]
              [(clojure.string/includes? ?content ".tif)")]
              [(clojure.string/includes? ?content ".bmp)")]
              [(clojure.string/includes? ?content ".svg)")]
              [(clojure.string/includes? ?content ".webp)")]
              [(clojure.string/includes? ?content ".PNG)")]
              [(clojure.string/includes? ?content ".JPG)")]
              [(clojure.string/includes? ?content ".JPEG)")]
              [(clojure.string/includes? ?content ".GIF)")]
              [(clojure.string/includes? ?content ".TIGG)")]
              [(clojure.string/includes? ?content ".TIF)")]
              [(clojure.string/includes? ?content ".VMP)")]
              [(clojure.string/includes? ?content ".SVG)")]
              [(clojure.string/includes? ?content ".WEBP)")])
        ]}
  #+END_QUERY
	- ../assets/image_1702339992213_0.png
	- ../assets/image_1702340054627_0.png
	- ../assets/image_1702343732906_0.png
	- ../assets/image_1702343809835_0.png
	- ../assets/image_1702393147540_0.png
	- ../assets/image_1702459387178_0.png
	- ../assets/image_1702461083537_0.png
	- ../assets/image_1702461103849_0.png
	- ../assets/image_1702461307665_0.png
	- ../assets/image_1702474167614_0.png
	- ../assets/image_1702474841334_0.png
	- ../assets/image_1702477252634_0.png
	- ../assets/image_1702477612765_0.png
	- ../assets/image_1702812601923_0.png
	- ../assets/image_1702815480279_0.png
	- ../assets/image_1702868177196_0.png
	- ../assets/image_1702869650032_0.png
	- ../assets/image_1702882649954_0.png
	- ../assets/image_1702882682434_0.png
	- ../assets/image_1702882745949_0.png
	- ../assets/image_1702882867898_0.png
	- ../assets/image_1702882916598_0.png
	- ../assets/image_1702882948005_0.png
	- ../assets/image_1702886082113_0.png
	- ../assets/image_1702886236714_0.png
	- ../assets/image_1702886428123_0.png
	- ../assets/image_1702886566382_0.png
	- ../assets/image_1702887081639_0.png
	- ../assets/image_1702887219880_0.png
	- ../assets/image_1702947242694_0.png
	- ../assets/image_1703577986616_0.png
	- ../assets/image_1703578464573_0.png
	- ../assets/image_1703578785081_0.png
	- ../assets/image_1703579151780_0.png
	- ../assets/image_1703580958140_0.png
	- ../assets/image_1703581690597_0.png
	- ../assets/image_1703581704543_0.png
	- ../assets/image_1703581879625_0.png
	- ../assets/image_1703581961071_0.png
	- ../assets/image_1703582072895_0.png
	- ../assets/image_1704335551867_0.png
	- ../assets/image_1704335626660_0.png
	- ../assets/image_1704336612801_0.png
	- ../assets/image_1704336652158_0.png
	- ../assets/image_1704336733396_0.png
	- ../assets/image_1704336790989_0.png
	- ../assets/image_1704336872510_0.png
	- ../assets/image_1704336970642_0.png
	- ../assets/image_1704337221629_0.png
	- ../assets/image_1704337286918_0.png
	- ../assets/image_1704338029409_0.png
	- ../assets/image_1704338109378_0.png
	- ../assets/image_1704338362546_0.png
	- ../assets/image_1704338477920_0.png
	- ../assets/image_1704338529054_0.png
	- ../assets/image_1704338620218_0.png
	- ../assets/image_1704338816041_0.png
	- ../assets/image_1704338930033_0.png
	- ../assets/image_1704339146343_0.png
	- ../assets/image_1704345574291_0.png
	- ../assets/image_1704345634165_0.png
	- ../assets/image_1704345725915_0.png
	- ../assets/image_1704346236396_0.png
	- ../assets/image_1704346293466_0.png
	- ../assets/image_1704347060339_0.png
	- ../assets/image_1704347447317_0.png
	- ../assets/image_1704347472109_0.png
	- ../assets/image_1704347600214_0.png
	- ../assets/image_1704347749760_0.png
	- ../assets/image_1704348545416_0.png
	- ../assets/image_1704348913719_0.png
	- ../assets/image_1704350496428_0.png
	- ../assets/image_1704353160120_0.png
	- ../assets/image_1704353225259_0.png
	- ../assets/image_1704353271905_0.png
	- ../assets/image_1704353390686_0.png
	- ../assets/image_1704353541450_0.png
	- ../assets/image_1704353870359_0.png
	- ../assets/image_1704354187095_0.png
	- ../assets/image_1704354393982_0.png
	- ../assets/image_1704508645879_0.png
	- ../assets/image_1704508863635_0.png
	- ../assets/image_1704509040682_0.png
	- ../assets/image_1704509418159_0.png
	- ../assets/image_1704510533651_0.png
	- ../assets/image_1704510959170_0.png
	- ../assets/image_1704512558416_0.png
	- ../assets/image_1704512718518_0.png
	- ../assets/image_1704512806852_0.png
	- ../assets/image_1704513128842_0.png
	- ../assets/image_1704513763665_0.png
	- ../assets/image_1704514217098_0.png
	- ../assets/image_1704530357415_0.png
	- ../assets/image_1704530673754_0.png
	- ../assets/image_1704530709643_0.png
	- ../assets/image_1704530731986_0.png
	- ../assets/image_1704530778304_0.png
	- ../assets/image_1704532688899_0.png
	- ../assets/image_1704532987307_0.png
	- ../assets/image_1704533137753_0.png
	- ../assets/image_1704533585463_0.png
	- ../assets/image_1704533716437_0.png
	- ../assets/image_1704534086806_0.png
	- ../assets/image_1704535898464_0.png
	- ../assets/image_1704536033644_0.png
	- ../assets/image_1704536384781_0.png
	- ../assets/image_1704545657264_0.png
	- ../assets/image_1704546234711_0.png
	- ../assets/image_1704546443815_0.png
	- ../assets/image_1704546672878_0.png
	- ../assets/image_1704547293762_0.png
	- ../assets/image_1704547948794_0.png
	- ../assets/image_1704548162477_0.png
	- ../assets/image_1704548447145_0.png
	- ../assets/image_1704548647763_0.png
	- ../assets/image_1704548815581_0.png
	- ../assets/image_1704548902998_0.png
	- ../assets/image_1704549140781_0.png
	- ../assets/image_1704591966072_0.png
	- ../assets/image_1704592065127_0.png
	- ../assets/image_1704592920178_0.png
	- ../assets/image_1704593009075_0.png
	- ../assets/image_1704593210160_0.png
	- ../assets/image_1704593717990_0.png
	- ../assets/image_1704593884052_0.png
	- ../assets/image_1704594025541_0.png
	- ../assets/image_1704594271179_0.png
	- ../assets/image_1704594672031_0.png
	- ../assets/image_1704594784102_0.png
	- ../assets/image_1704594893180_0.png
	- ../assets/image_1704594990621_0.png
	- ../assets/image_1704595338391_0.png
	- ../assets/image_1704595549550_0.png
	- ../assets/image_1704595653801_0.png
	- ../assets/image_1704595769616_0.png
	- ../assets/image_1704595844978_0.png
	- ../assets/image_1704596002337_0.png
	- ../assets/image_1705314320332_0.png
	- ../assets/image_1705315658364_0.png
	- ../assets/image_1705550747014_0.png
	- ../assets/image_1705550964372_0.png
	- ../assets/image_1705552177057_0.png
	- ../assets/image_1705552540935_0.png
	- ../assets/image_1705552569825_0.png
	- ../assets/image_1705552655073_0.png
	- ../assets/image_1705552793718_0.png
	- ../assets/image_1705552956967_0.png
	- ../assets/image_1705553204828_0.png
	- ../assets/image_1705553274420_0.png
	- ../assets/image_1705553355005_0.png
	- ../assets/image_1705553556544_0.png
	- ../assets/image_1705553620363_0.png
	- ../assets/image_1705568431547_0.png
	- ../assets/image_1705568902726_0.png
	- ../assets/image_1705634829440_0.png
	- ../assets/image_1705634901981_0.png
	- ../assets/image_1705635099423_0.png