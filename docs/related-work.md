# 先行研究

NotebookLM（「表記ゆれ×多言語埋め込み検索(Google SR 2027)」、12本）の比較（2026-10-07）を元にしたもの。
**NotebookLM の要約なので、本文で確かめてから引用する**。確かめたら「確認」の欄に日付を書く。

## 直接比べる3本

| 論文 | 試した変化 | 言語 | モデル・データ | 指標 | 主な結果 | 確認 |
|---|---|---|---|---|---|---|
| 佐々木・山本「表記ゆれが文埋め込みモデルに及ぼす影響についての考察」（言語処理学会 2025、P8-5、CC BY 4.0） | Sudachi の同義語辞書にある語を、ランダムに同義語へ書き換え（同義語が見つからないクエリは除外）。図1の例は全角半角・「東京大学→とうきょう大学」・「申し込み→申込み」・「不具合→バグ」だが、**種類別には分けていない** | 日本語 | multilingual-e5-base/large、ruri-base/large ／ JMTEB の JAQKET（validation）・JaGovFaqs-22k。**BM25 / TF-IDF は実験に入っていない** | Top-5 に正解が入るクエリ数の XOR / AND の比（nDCG ではない） | XOR/AND は 0.0575（e5-large, JAQKET）〜 0.1406（ruri-base, JAQKET）。「ヒットのずれは 5%〜15%程度」「large の方が頑健」「ruri の方がやや影響が大きい」。{元|同義語} を並べて書くと AND が増える。6章：「同義語辞書以外の表記ゆれ（送り仮名揺れや外来語カタカナ表記揺れなど）に対しても同様の検証を行う予定」。2章：全角半角・送り仮名は「前処理によってある程度吸収することが可能な場合が多い」 | 2026-10-09 本文確認 |
| Chari, Ounis, MacAvaney "Lost in Transliteration"（SIGIR 2025、arXiv:2505.08411） | ローマ字化（Greeklish、Arabizi など） | ギリシャ語・アラビア語など。日本語なし | BGE-M3 など | 本文で確認 | ローマ字化で精度が急落。母語の文字とローマ字を半々で混ぜて fine-tune するとほぼ回復 | |
| Zhuang & Zuccon "Dealing with Typos for BERT-based Passage Retrieval and Ranking"（EMNLP 2021） | 誤字 | 英語 | 密ベクトル検索、BERT リランカー ／ MS MARCO | 本文で確認 | 誤字で大きく低下。誤字入りで学習すると回復 | |

## この研究との違い（言語処理学会 2025 は 2026-10-09 本文確認。他の2本は要確認）
- 同じ日本語の語の**書き方だけ**を、**種類別に分けて**測った研究は3本ともない
- 言語処理学会 2025 は同義語と表記ゆれが混ざった書き換えで、種類ごとの影響を分けていない（本文で確認）
- 言語処理学会 2025 は文字一致の検索（BM25 / TF-IDF）と並べて測っていない。「埋め込みで解けたのか」に答えるには、BM25 と NFKC 正規化の比較が要る
- 言語処理学会 2026（NLP2026）のプログラムに、続きにあたる発表は見当たらない（題名で検索。2026-10-09）
- 実際の商品検索のクエリ（ESCI）で測った研究はない
- LLM ベースの埋め込みモデルは3本とも評価していない

## 2026 年に見つけた近い研究（要旨だけ確認）
- Mibayashi & Ohshima "Evaluating the Robustness of Japanese LLMs to IME-Related and Typographical Errors"（arXiv:2610.01241、2026-10-01）：11 の LLM、JMMLU・JCommonsenseQA・JamC-QA。5種類（文字の入れ替え、文字の置き換え、同音異義語、IME 変換、全角変換）。入れ替えと置き換えでは一貫して下がり、IME 変換・全角変換・同音は「relatively limited impact」。**検索・埋め込みは扱っていない**。本文は要確認

## その他（ノートブックに入っているもの）
- Zhuang & Zuccon, CharacterBERT and Self-Teaching（arXiv:2204.00716）：誤字に強くする学習
- Typo-Robust Representation Learning for Dense Retrieval（ACL 2023 short）
- On the Robustness of LLM-Based Dense Retrievers（arXiv:2604.16576）：LLM ベースの検索器の頑健性
- Arnett ほか, Evaluating Morphological Alignment of Tokenizers in 70 Languages（arXiv:2507.06378）：形態素との一致は性能差をあまり説明しない
- Arnett & Bergen, Why do language models perform worse for morphologically complex languages?（arXiv:2411.14198）
- Decompounding query keywords from compounding languages（ACL 2008）：ドイツ語の複合語
- NII 技術報告 05-011E、CJKI：日本語の表記ゆれの分類
- JMTEB（Hugging Face のデータセット）

## 背景（埋め込み以前の取り組み）
- Microsoft Research「Discovery of Term Variation in Japanese Web Search Queries」：日本語のウェブ検索クエリから表記ゆれを見つける
- カタカナの揺れを認識する辞書引きの研究
→ 「辞書と単語一致の時代に手当てしていた問題が、埋め込みで自然に解けたのか」という問いの背景になる
