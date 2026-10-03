# Memo Teardown — NotebookLM (nay là Gemini Notebook)

**Họ tên:** Trần Thị Thuý  
**Mã sinh viên:** 2A202602960  
**Ngày hoàn thiện:** 03/10/2026  

**Vì sao chọn sản phẩm này:** NotebookLM là một sản phẩm AI-native: mô hình ngôn ngữ không chỉ được gắn thêm vào một phần mềm ghi chú có sẵn mà là lõi của toàn bộ trải nghiệm đọc, hỏi đáp, tổng hợp và tạo đầu ra. Sản phẩm cũng có lịch sử công khai đủ dài, use case rõ và nhiều quyết định lớn để quan sát quá trình đi từ một trợ lý đọc tài liệu thử nghiệm thành một nền tảng nghiên cứu đa phương thức trong hệ sinh thái Google.

> **Lưu ý tên gọi:** Google đổi tên NotebookLM thành **Gemini Notebook** ngày 16/07/2026. Memo vẫn dùng tên NotebookLM khi nói về lịch sử trước thời điểm này và dùng Gemini Notebook khi nói về trạng thái hiện tại.

## §1. Timeline các cập nhật lớn

| Thời điểm | Cập nhật | Context lúc đó | Nguyên lý |
|---|---|---|---|
| **12/07/2023** | Google Labs đổi tên Project Tailwind thành **NotebookLM** và mở thử nghiệm cho một nhóm nhỏ tại Mỹ. Điểm khác biệt cốt lõi là mô hình được *ground* trong Google Docs do người dùng chọn, trả lời kèm trích dẫn thay vì dựa chủ yếu vào kiến thức chung. [Nguồn: Google — Introducing NotebookLM](https://blog.google/innovation-and-ai/technology/ai/notebooklm-google-ai/) | Sau làn sóng ChatGPT, chatbot tổng quát tăng rất nhanh nhưng rủi ro bịa thông tin và thiếu ngữ cảnh riêng vẫn lớn. Nhóm người dùng đầu tiên — sinh viên, giảng viên, tác giả và knowledge worker — đã có tài liệu nhưng mất nhiều thời gian nối các ý giữa chúng. | **Vertical AI + định nghĩa “tốt”:** Google không tối ưu cho “biết mọi thứ” mà đóng phạm vi vào kho nguồn người dùng chọn. “Tốt” được định nghĩa là câu trả lời hữu ích, có căn cứ và có thể lần ngược về nguồn. Đây là wedge đủ hẹp để khác chatbot tổng quát. |
| **08/12/2023** | NotebookLM mở cho người dùng Mỹ từ 18 tuổi, chuyển sang Gemini Pro và bổ sung noteboard, lưu câu trả lời thành note, suggested actions, tạo outline/study guide và xuất sang Google Docs. [Nguồn: Google — More than a dozen new features](https://blog.google/innovation-and-ai/products/notebooklm-new-features-availability/) | Feedback từ nhóm thử nghiệm cho thấy tóm tắt và gợi ý câu hỏi có ích, nhưng hỏi–đáp đơn lẻ chưa hoàn thành công việc. Người dùng vẫn phải chuyển qua công cụ khác để ghi chép, tổ chức và viết đầu ra. | **Wrapper → workflow/moat:** sản phẩm bắt đầu bao trọn vòng lặp *đọc nguồn → hỏi → lưu ý → tổ chức → viết*, thay vì chỉ là giao diện chat bọc quanh LLM. Mỗi bước giữ thêm context trong notebook và làm lần tương tác sau có giá trị hơn. |
| **06/06/2024** | Dùng Gemini 1.5 Pro, mở tại hơn 200 quốc gia/vùng lãnh thổ; nhận Google Slides và URL web, hiểu hình/biểu đồ, thêm inline citation dẫn thẳng tới đoạn gốc và Notebook Guide tạo FAQ, Briefing Doc, Study Guide. [Nguồn: Google — NotebookLM goes global](https://blog.google/innovation-and-ai/products/notebooklm-goes-global-support-for-websites-slides-fact-check/) | Long context và multimodal đã tiến bộ; nhu cầu không còn giới hạn ở Google Docs hay người dùng Mỹ. Muốn trở thành công cụ nghiên cứu thật, sản phẩm phải xử lý đúng những định dạng mà người dùng đã có và giảm chi phí kiểm chứng. | **x10 + định nghĩa “tốt”:** mở rộng loại nguồn và phạm vi địa lý tạo bước nhảy về khả năng dùng, nhưng inline citation giữ tiêu chuẩn cốt lõi là *grounded và kiểm chứng được*. Đây không chỉ là model tốt hơn; UX kiểm chứng được thiết kế thành một phần của sản phẩm. |
| **11–26/09/2024** | Ra mắt **Audio Overviews** — hai host AI biến nguồn thành cuộc trò chuyện có thể tải xuống; hai tuần sau hỗ trợ YouTube, audio file và link chia sẻ Audio Overview. [Nguồn: Google — Audio Overviews](https://blog.google/innovation-and-ai/products/notebooklm-audio-overviews/) · [Nguồn: Google — YouTube/audio sources](https://blog.google/innovation-and-ai/products/notebooklm-audio-video-sources/) | Q&A và summary bằng chữ đang dần thành tính năng phổ thông của mọi chatbot. Trong khi đó, nhiều người muốn học lúc di chuyển hoặc tiếp nhận tài liệu dài mà không phải nhìn màn hình. | **x10 trong trải nghiệm + vòng lặp học:** Audio Overview không chỉ trả lời nhanh hơn mà đổi hẳn modality từ đọc sang nghe. Đầu ra dễ chia sẻ tạo vòng lặp *nguồn → nội dung hấp dẫn → chia sẻ → người mới thử sản phẩm*, đồng thời tạo signature feature khó bị nhầm với chatbot tài liệu thông thường. |
| **13/12/2024** | Thiết kế lại thành ba vùng **Sources – Chat – Studio**, cho phép “tham gia” cuộc trò chuyện với host Audio Overview và ra mắt **NotebookLM Plus** cho power user, team, trường học và doanh nghiệp với hạn mức cao, notebook dùng chung, analytics, quyền riêng tư và bảo mật bổ sung. [Nguồn: Google — New look, interactive audio and Plus](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-new-features-december-2024/) | Chỉ ba tháng sau khi Audio Overviews ra mắt, người dùng đã tạo hơn 350 năm nội dung audio. Google có tín hiệu PMF ở cả cá nhân lẫn tổ chức, nhưng giao diện cũ chưa phản ánh ba công việc khác nhau: quản nguồn, đối thoại và tạo artifact. | **Workflow moat + mô hình kiếm tiền:** cấu trúc ba panel biến chuỗi thao tác thành một “research operating system”; Plus thu tiền từ cường độ sử dụng, cộng tác và governance thay vì khóa giá trị cơ bản. Shared notebooks và tổ chức nguồn bắt đầu tạo switching cost ở cấp đội nhóm. |
| **02/04–19/05/2025** | Bổ sung **Discover Sources** để tìm và nhập nguồn web; sau đó ra mắt app iOS/Android với nghe offline, hỏi host và chia sẻ PDF/web/YouTube trực tiếp vào notebook. [Nguồn: Google — Discover Sources](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-discover-sources/) · [Nguồn: Google — Mobile app](https://blog.google/innovation-and-ai/products/notebooklm-app/) | Nút thắt của phiên bản trước là người dùng phải có sẵn nguồn, tự thu gom rồi mới nhận giá trị. Thành công của Audio Overviews cũng tạo nhu cầu dùng trong lúc di chuyển; mobile là yêu cầu được người dùng nhắc nhiều. | **Giảm time-to-value + vòng lặp học:** Discover Sources kéo điểm bắt đầu về từ “đã có bộ tài liệu” thành “mới có một chủ đề”. Mobile đóng vòng lặp *gặp nguồn ở bất kỳ đâu → lưu vào notebook → hiểu bằng chat/audio → tiếp tục bổ sung*, làm sản phẩm hiện diện trong thói quen hằng ngày. |
| **29/07–13/11/2025** | Ra mắt **Video Overviews**, nâng Studio để tạo nhiều artifact cùng loại; tiếp đó thêm **Deep Research** và nguồn Google Sheets, ảnh, Drive PDF, DOCX. [Nguồn: Google — Video Overviews & Studio](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-video-overviews-studio-upgrades/) · [Nguồn: Google — Deep Research & new file types](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-deep-research-file-types/) | Thị trường chuyển từ chatbot sang agent; người dùng kỳ vọng AI không chỉ giải thích kho tài liệu có sẵn mà còn tự tìm, đánh giá và tổ chức nguồn. Đồng thời, một nguồn có thể cần nhiều đầu ra cho các vai trò, ngôn ngữ hoặc chương khác nhau. | **Wrapper → compound product:** NotebookLM mở rộng từ “hỏi tài liệu” sang cả hai phía của workflow: agent thu thập nguồn ở đầu vào và Studio tạo nhiều artifact ở đầu ra. Moat nằm ở sự phối hợp giữa repository, provenance, reasoning và output — không chỉ ở model. |
| **08/06–16/07/2026** | Notebook có secure cloud computer để chạy code, hơn 100 software skills, tạo PDF/DOCX/Markdown, chart, CSV/JSON, Excel và PowerPoint; sau đó Google đổi tên sản phẩm thành **Gemini Notebook**, đồng bộ với Gemini app và công bố kế hoạch đưa notebook vào AI Mode của Search. Google cho biết sản phẩm đã có hơn 30 triệu người dùng và 600.000 tổ chức. [Nguồn: Google — Better research](https://blog.google/innovation-and-ai/products/notebooklm/better-research-notebooklm/) · [Nguồn: Google — NotebookLM is now Gemini Notebook](https://blog.google/innovation-and-ai/products/gemini-notebook/notebooklm-gemini-notebook/) | Model tổng quát đã có project/file upload, nên lợi thế “chat với tài liệu” bị thu hẹp. Cạnh tranh dịch sang agent có thể nghiên cứu, chạy công cụ và tạo deliverable hoàn chỉnh. Google đồng thời có lợi thế phân phối qua Gemini, Search và Workspace. | **Platform moat + x10:** secure computer biến notebook từ nơi *đọc và hiểu* thành nơi *nghiên cứu, tính toán và sản xuất deliverable*. Đổi tên và tích hợp hệ sinh thái cho thấy chiến lược biến notebook thành lớp context có thể tái sử dụng trên các bề mặt Google — một moat về distribution và workflow, không phải độc quyền model. |

### Vì sao chọn những mốc này?

Tôi chọn các mốc làm thay đổi ít nhất một trong bốn biến: **giá trị cốt lõi, tệp người dùng, workflow hoặc mô hình kinh doanh**. Ví dụ, Audio Overviews tạo một cách tiêu thụ hoàn toàn mới và Plus mở tệp team/doanh nghiệp; đây là quyết định sản phẩm, không chỉ là bản nâng cấp kỹ thuật.

Tôi đã cân nhắc nhưng loại riêng các mốc như mở thêm ngôn ngữ, thêm một kiểu hình cho Video Overview, thay đổi độ dài audio hoặc hỗ trợ thêm một định dạng file. Chúng có ích nhưng chủ yếu mở rộng quyết định lớn đã có. Tôi cũng không tách Project Tailwind tại I/O 2023 thành một hàng riêng vì đợt NotebookLM tháng 07/2023 mới là lúc định vị source-grounded được công bố thành sản phẩm có thể thử.

## §2. Tệp user & JTBD

| | Early adopters (2023–đầu 2024) | Tệp hiện tại (2026) |
|---|---|---|
| **Đặc điểm** | Sinh viên sau đại học, giảng viên, tác giả, nhà báo/người làm podcast và knowledge worker tại Mỹ; đã có một tập Google Docs hoặc PDF lớn cho một dự án; sẵn sàng thử công cụ Google Labs và chấp nhận tính thử nghiệm để đổi lấy khả năng tổng hợp nhanh. Một người cụ thể có thể là nghiên cứu sinh đang đọc 20 bài báo cho luận văn hoặc tác giả đang đối chiếu hàng trăm trang tư liệu. | Sinh viên/người nghiên cứu, program manager, data analyst, chủ doanh nghiệp nhỏ và team trong tổ chức; làm việc với nhiều loại file, web và dữ liệu; thường đã dùng Drive/Workspace/Gemini; cần vừa tìm nguồn, phân tích, tạo artifact, chia sẻ và sử dụng trên web/mobile. Đến 07/2026, Google công bố hơn **30 triệu người dùng và 600.000 tổ chức**. |
| **JTBD chính** | “Khi phải hiểu một tập tài liệu phức tạp mà tôi đã tin cậy, hãy giúp tôi nối ý, trả lời câu hỏi và tạo bản nháp có trích dẫn để tôi không phải tìm thủ công từng tài liệu.” | “Khi bắt đầu từ một câu hỏi hoặc dự án còn mơ hồ, hãy giúp tôi xây kho nguồn đáng tin, nghiên cứu/phân tích nó và biến kết quả thành đầu ra đúng định dạng cho học tập hoặc công việc — mà vẫn thấy được nguồn gốc.” |
| **Trước đó họ làm bằng cách nào?** | Mở nhiều tab/PDF, Ctrl+F từng tài liệu, ghi chú vào Docs/Notion/Zotero rồi tự viết synthesis; hoặc copy-paste từng đoạn vào chatbot tổng quát và tự truy ngược nguồn. | Ghép nhiều công cụ: Google Search/Deep Research để tìm nguồn, Drive/Notion để lưu, ChatGPT/Claude để hỏi, podcast/video generator để chuyển modality, Sheets/Python để phân tích và Slides/Docs để đóng gói. Handoff giữa công cụ làm mất context và provenance. |
| **Cột mốc gây dịch chuyển** | Mốc 07/2023 và 12/2023 tạo wedge cho nhóm có sẵn tài liệu và cần citation. | Mốc 06/2024 mở toàn cầu; 09/2024 đưa sản phẩm sang học/nghe; 12/2024 mở Plus cho tổ chức; 04–05/2025 bỏ yêu cầu “phải có sẵn nguồn” và đưa sản phẩm lên mobile; 06–07/2026 mở phân tích bằng code, artifact và phân phối qua hệ sinh thái Gemini. |

### Dịch chuyển tệp

Ban đầu NotebookLM yêu cầu người dùng vừa **có sẵn corpus** vừa có đủ động lực để tự tổ chức corpus đó. Vì vậy early adopters nghiêng về researcher, writer và student có dự án dài. Discover Sources và Deep Research giảm công việc thu gom; Audio/Video Overviews và mobile mở tình huống tiêu thụ hằng ngày; Plus, shared notebooks và secure cloud computer mở các công việc có ngân sách và governance ở tổ chức. Chuỗi mốc này dịch sản phẩm từ “công cụ đọc tài liệu cá nhân” sang “workspace nghiên cứu và sản xuất đầu ra cho cả cá nhân lẫn team”.

### Switching cost — map theo 4 forces

| Force | Biểu hiện ở NotebookLM/Gemini Notebook |
|---|---|
| **Push — bất mãn với cách cũ** | Tài liệu nằm rải rác; phải tìm và đối chiếu thủ công; chatbot tổng quát có thể bịa và làm mất provenance; phải copy context qua nhiều công cụ để tạo báo cáo, audio, video hay slide. |
| **Pull — sức hút của cách mới** | Một repository source-grounded có citation; cùng một nguồn có thể biến thành chat, mind map, audio, video, báo cáo, bảng tính và slide; mobile và Gemini/Search giảm ma sát truy cập. Giá trị không chỉ là câu trả lời mà là cả chuỗi hoàn thành công việc. |
| **Anxiety — lo ngại khi chuyển sang** | AI vẫn có thể diễn giải sai; Deep Research có thể chọn nguồn kém; dữ liệu nhạy cảm và quyền truy cập trong tổ chức cần được quản trị; phụ thuộc vào sản phẩm/hạn mức Google; output hấp dẫn có thể khiến người dùng tin quá mức. |
| **Habit/Inertia — lực giữ cách cũ** | Người dùng đã quen với Search, Docs, Notion, Zotero, ChatGPT hoặc Claude; nguồn gốc vẫn có thể lưu ở Drive và file cục bộ nên chi phí rời đi ở cấp cá nhân chưa tuyệt đối cao. Ở cấp team, notebook dùng chung, cấu trúc nguồn, artifact, analytics và quyền quản trị làm lực giữ mạnh hơn. |

**Lực giữ user mạnh nhất:** dữ liệu đơn thuần chưa phải moat vì phần lớn nguồn có thể tải hoặc nhập lại. Lực mạnh nhất là **workflow tích lũy quanh repository**: cách team chọn nguồn, các notebook dùng chung, lịch sử câu hỏi, artifact cho từng đối tượng và phân phối qua Gemini/Workspace. Nếu đối thủ nhập nguyên notebook kèm citation và artifact chỉ bằng một lần bấm, switching cost sẽ giảm mạnh; Google khi đó phải dựa nhiều hơn vào chất lượng grounded reasoning và lợi thế phân phối.

## §3. Ba dự đoán hướng đi (6–12 tháng tới)

### Dự đoán 1 *(loại: mở rộng tính năng / đe dọa từ Big Tech)*

- **Dự đoán:** Gemini Notebook sẽ trở thành lớp context dùng chung sâu hơn giữa Gemini app, AI Mode trong Search và Workspace; người dùng có thể gọi một notebook trực tiếp trong Docs, Slides, Sheets hoặc một Gemini workflow mà không phải mở sản phẩm độc lập.
- **Lập luận:** Mốc 07/2026 đã đổi tên, đồng bộ notebook với Gemini app và công bố hướng đưa vào Search; timeline trước đó cũng cho thấy chiến lược liên tục giảm handoff giữa nguồn, reasoning và artifact. Tích hợp là cách Google dùng distribution moat trước khi các chatbot tổng quát sao chép hết tính năng “chat với tài liệu”.

### Dự đoán 2 *(loại: mở rộng segment)*

- **Dự đoán:** Google sẽ đẩy Gemini Notebook thành workspace nghiên cứu có governance cho team/doanh nghiệp, bổ sung template/skill theo vai trò, quyền nguồn chi tiết, audit trail, phê duyệt artifact và kết nối kho dữ liệu nội bộ.
- **Lập luận:** Plus năm 2024 đã thử nghiệm shared notebook, analytics và enterprise protection; đến 2026 đã có 600.000 tổ chức, secure cloud computer và hơn 100 skill. Bước hợp lý tiếp theo là đóng gói năng lực này thành workflow lặp lại cho analyst, program manager, onboarding và knowledge management thay vì chỉ phục vụ notebook cá nhân.

### Dự đoán 3 *(loại: thay đổi mô hình kiếm tiền)*

- **Dự đoán:** Giá sẽ phân tầng rõ hơn theo lượng compute/agent task và loại artifact: bản miễn phí giữ chat/citation cơ bản, Pro mở hạn mức nghiên cứu và tạo nội dung, còn Ultra/Workspace bán tác vụ chạy code, video chất lượng cao, automation và governance.
- **Lập luận:** Plus ban đầu kiếm tiền bằng hạn mức cao và tính năng team; các mốc 2025–2026 bổ sung Deep Research, video, cloud computer và nhiều định dạng đầu ra có chi phí không đồng đều. Thu phí theo năng lực/compute phù hợp hơn một gói “nhiều notebook hơn”, đồng thời bảo vệ free tier để duy trì vòng lặp thu hút người dùng.

**Dự đoán tự tin nhất:** Dự đoán 1, vì Google đã công bố hướng tích hợp với Gemini app và Search. Giả định có thể làm dự đoán gãy là người dùng phản đối việc trộn notebook riêng tư vào các bề mặt Google hoặc Google giữ Gemini Notebook như sản phẩm độc lập để bảo toàn niềm tin và sự tập trung.

## §4. AI Log

| Việc | AI làm hay bạn làm? | Bạn kiểm chứng/phán đoán lại thế nào? |
|---|---|---|
| Chọn sản phẩm NotebookLM | **Bạn làm** | Người học chủ động chọn NotebookLM. AI đối chiếu với ba tiêu chí của lab: AI là lõi trải nghiệm, có hơn 6 mốc công khai và JTBD source-grounded rõ. |
| Tìm và sắp xếp nguồn thô | **AI làm phần lớn** | AI tìm và mở các bài gốc có ngày phát hành trên Google Blog cùng trang Product Hunt; chỉ giữ mốc có URL nguồn. Trước khi nộp, người học cần mở lại tối thiểu ba link và kiểm tra ngày/phạm vi rollout. |
| Chọn 8 mốc và viết context | **AI đề xuất; bạn cần duyệt** | AI loại các cập nhật nhỏ và giải thích tiêu chí chọn ngay dưới bảng. Người học cần xác nhận đồng ý với việc gộp các đợt 09/2024, 04–05/2025, 07–11/2025 và 06–07/2026 thành bốn quyết định chiến lược. |
| Revert mốc về nguyên lý | **AI viết bản đầu** | Mỗi nhãn được nối với bằng chứng trong chính quyết định: source-grounding → Vertical AI/định nghĩa “tốt”; ba panel/Plus → workflow moat; mobile/Discover → vòng lặp; cloud computer/ecosystem → platform moat. Người học cần bảo đảm tên framework khớp cách gọi trong buổi học. |
| Phân tích user, JTBD và 4 forces | **AI tổng hợp và phán đoán** | Các tệp user được neo vào nhóm người dùng/case được Google công bố; phần switching cost được tách thành dữ liệu có thể mang đi và workflow khó tái tạo để tránh phóng đại moat. Người học nên thử một notebook thực tế để bổ sung quan sát cá nhân nếu có. |
| Viết ba dự đoán 6–12 tháng | **AI đề xuất; bạn chịu trách nhiệm chọn** | Lập luận chỉ dùng tín hiệu đã xuất hiện trong timeline; mỗi dự đoán nêu một giả định có thể sai. Người học cần đọc và chỉ giữ các nhận định mình có thể tự bảo vệ khi bị hỏi phản biện. |
| Biên tập memo theo template | **AI làm** | AI kiểm tra đủ bốn phần, 8 mốc, link nguồn, 3 dự đoán và AI log. Người học cần đọc toàn văn, sửa giọng văn nếu cần và xác nhận AI log phản ánh đúng mức sử dụng thực tế trước khi đưa lên repo. |

## Phụ lục — Nguồn thô và các mốc ứng viên đã mở

1. [Introducing NotebookLM — 12/07/2023](https://blog.google/innovation-and-ai/technology/ai/notebooklm-google-ai/)
2. [NotebookLM adds more than a dozen new features — 08/12/2023](https://blog.google/innovation-and-ai/products/notebooklm-new-features-availability/)
3. [NotebookLM goes global — 06/06/2024](https://blog.google/innovation-and-ai/products/notebooklm-goes-global-support-for-websites-slides-fact-check/)
4. [Audio Overviews — 11/09/2024](https://blog.google/innovation-and-ai/products/notebooklm-audio-overviews/)
5. [YouTube/audio sources and sharing — 26/09/2024](https://blog.google/innovation-and-ai/products/notebooklm-audio-video-sources/)
6. [New interface, interactive audio and NotebookLM Plus — 13/12/2024](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-new-features-december-2024/)
7. [Product Hunt: Google NotebookLM Plus — launch 15/12/2024](https://www.producthunt.com/products/google-notebooklm-plus)
8. [Discover Sources — 02/04/2025](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-discover-sources/)
9. [Mind Maps and study features — 03/04/2025](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-studying-help/)
10. [NotebookLM mobile app — 19/05/2025](https://blog.google/innovation-and-ai/products/notebooklm-app/)
11. [Product Hunt: Google NotebookLM mobile](https://www.producthunt.com/products/google-notebooklm-2)
12. [Video Overviews and upgraded Studio — 29/07/2025](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-video-overviews-studio-upgrades/)
13. [Inside story of building NotebookLM — 29/07/2025](https://blog.google/innovation-and-ai/products/developing-notebooklm/)
14. [Video Overviews in 80 languages — 25/08/2025](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebook-lm-audio-video-overviews-more-languages-longer-content/)
15. [Video Overviews with Nano Banana — 13/10/2025](https://blog.google/innovation-and-ai/models-and-research/google-labs/video-overviews-nano-banana/)
16. [Deep Research and more source types — 13/11/2025](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-deep-research-file-types/)
17. [Cinematic Video Overviews — 04/03/2026](https://blog.google/innovation-and-ai/products/notebooklm/generate-your-own-cinematic-video-overviews-in-notebooklm/)
18. [Better research: cloud computer, code and artifact formats — 08/06/2026](https://blog.google/innovation-and-ai/products/notebooklm/better-research-notebooklm/)
19. [NotebookLM is now Gemini Notebook — 16/07/2026](https://blog.google/innovation-and-ai/products/gemini-notebook/notebooklm-gemini-notebook/)

---

**Checklist trước khi nộp**

- [x] Timeline có 8 mốc, mỗi mốc là quyết định sản phẩm và có link nguồn.
- [x] Mỗi mốc được revert về nguyên lý có tên.
- [x] Early adopters và tệp hiện tại đủ cụ thể; JTBD viết theo việc cần làm.
- [x] Segment shift nối với các mốc ở §1; switching cost được map theo 4 forces.
- [x] Có 3 dự đoán, mỗi dự đoán có lập luận dẫn về timeline/user.
- [x] AI log có hơn 3 hàng và không che giấu phần AI đã làm.
- [ ] Người học đã tự mở tối thiểu 3 nguồn, đọc toàn bài và xác nhận các nhận định.
- [ ] Repo Public được đặt tên `Day16-Track1-2A202602960-TranThiThuy` và đã upload `memo.md`.
