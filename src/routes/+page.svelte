<script lang="ts">
    type Region = { id: string; label: string; icon: string };
    type Topic = { id: string; name: string; gradient: string };
    type Writer = {
        id: string;
        name: string;
        firstName: string;
        lastName: string;
        gradient: string;
    };
    type ArticleAttachment = {
        name: string;
        type: string;
        size: number;
        file: File;
    };

    type Article = {
        id: number;
        title: string;
        excerpt: string;
        body: string[];
        region: string;
        topic: string;
        topicLabel: string;
        author: string;
        team: string;
        published: string;
        readTime: string;
        likes: number;
        liked: boolean;
        verified: boolean;
        verifier: string;
        verifiedAt: string;
        sources: string[];
        resources?: string[];
        attachments?: ArticleAttachment[];
        factCheckFiles?: ArticleAttachment[];
        gradient: string;
        deleted?: boolean;
    };

    const regions: Region[] = [
        { id: "na", label: "North America", icon: "🦅" },
        { id: "sa", label: "South America", icon: "🦜" },
        { id: "eu", label: "Europe", icon: "🦌" },
        { id: "af", label: "Africa", icon: "🐘" },
        { id: "asia", label: "Asia", icon: "🐉" },
        { id: "oc", label: "Oceania", icon: "🐳" },
        { id: "global", label: "World", icon: "🌐" },
        { id: "originals", label: "Originals", icon: "✍️" },
        { id: "writers", label: "Writers", icon: "👥" },
        { id: "create", label: "Create a post", icon: "✏️" },
        { id: "account", label: "Account", icon: "👤" },
        { id: "settings", label: "Settings", icon: "⚙️" },
    ];

    const regionNames: Record<string, string> = {
        global: "World",
        na: "North America",
        sa: "South America",
        eu: "Europe",
        af: "Africa",
        asia: "Asia",
        oc: "Oceania",
        originals: "Foews Originals",
        account: "Account",
        settings: "Settings",
        create: "Create a post",
        writers: "Writers",
    };

    const regionSlogans: Record<string, string> = {
        global: "Technology without the algorithmic noise.",
        na: "From silicon laboratories to the software frontier.",
        sa: "New engineering ideas from a rapidly changing continent.",
        eu: "Systems, standards, and the engineers behind them.",
        af: "Discovering the people building Africa's technological future.",
        asia: "Where hardware, software, and scale meet.",
        oc: "Independent engineering from across the Pacific.",
    };

    const helloSnippets: Record<
        string,
        { language: string; filename: string; code: string }
    > = {
        global: {
            language: "Java",
            filename: "HelloWorld.java",
            code: 'public class HelloWorld {\n    public static void main(String[] args) {\n        System.out.println("Hello, World!");\n    }\n}',
        },
        na: {
            language: "C++",
            filename: "hello.cpp",
            code: '#include <iostream>\n\nint main() {\n std::cout << "Hello, world!\\n";\n return 0;\n}',
        },
        sa: {
            language: "Python",
            filename: "hello.py",
            code: 'def main():\n message = "Hello, world!"\n print(message)\n\nif __name__ == "__main__":\n main()',
        },
        eu: {
            language: "Rust",
            filename: "main.rs",
            code: 'fn main() {\n let message = "Hello, world!";\n println!("{}", message);\n}',
        },
        af: {
            language: "C",
            filename: "hello.c",
            code: '#include <stdio.h>\n\nint main(void) {\n puts("Hello, world!");\n return 0;\n}',
        },
        asia: {
            language: "Go",
            filename: "main.go",
            code: 'package main\n\nimport "fmt"\n\nfunc main() {\n fmt.Println("Hello, world!")\n}',
        },
        oc: {
            language: "JavaScript",
            filename: "hello.js",
            code: 'function main() {\n const message = "Hello, world!";\n console.log(message);\n}\n\nmain();',
        },
        originals: {
            language: "Swift",
            filename: "FoewsOriginals.swift",
            code: 'struct FoewsOriginals {\n static let mission = "Independent journalism."\n\n static func publish() {\n print(mission)\n }\n}\n\nFoewsOriginals.publish()',
        },
        writers: {
            language: "x86-64",
            filename: "hello.asm",
            code: [
                "global _start",
                "section .text",
                "_start:",
                "    mov eax, 1",
                "    mov edi, 1",
                "    lea rsi, [rel message]",
                "    mov edx, message_size",
                "    syscall",
                "    mov eax, 60",
                "    xor edi, edi",
                "    syscall",
                "section .rodata",
                'message: db "Hello, world!", 10',
                "message_size equ $ - message",
            ].join("\n"),
        },
    };

    const topicCatalog: Record<string, Topic[]> = {
        global: [
            {
                id: "languages",
                name: "Languages",
                gradient: "linear-gradient(125deg,#f9c74f,#f941a5)",
            },
            {
                id: "systems",
                name: "Systems",
                gradient: "linear-gradient(125deg,#00b4d8,#4361ee)",
            },
            {
                id: "hacks",
                name: "Hacks",
                gradient: "linear-gradient(125deg,#f3722c,#f9c74f)",
            },
            {
                id: "processors",
                name: "Processors",
                gradient: "linear-gradient(125deg,#7209b7,#f72585)",
            },
            {
                id: "security",
                name: "Security",
                gradient: "linear-gradient(125deg,#43aa8b,#90be6d)",
            },
            {
                id: "ai",
                name: "AI",
                gradient: "linear-gradient(125deg,#3a0ca3,#4cc9f0)",
            },
        ],
        na: [
            {
                id: "languages",
                name: "Languages",
                gradient: "linear-gradient(125deg,#f94144,#f9c74f)",
            },
            {
                id: "systems",
                name: "Systems",
                gradient: "linear-gradient(125deg,#277da1,#90e0ef)",
            },
            {
                id: "silicon",
                name: "Silicon",
                gradient: "linear-gradient(125deg,#6930c3,#80ffdb)",
            },
            {
                id: "cybersecurity",
                name: "Cybersecurity",
                gradient: "linear-gradient(125deg,#386641,#a7c957)",
            },
            {
                id: "ai",
                name: "AI",
                gradient: "linear-gradient(125deg,#4361ee,#f72585)",
            },
            {
                id: "cloud",
                name: "Cloud",
                gradient: "linear-gradient(125deg,#f896d8,#4cc9f0)",
            },
        ],
        sa: [
            {
                id: "programming",
                name: "Programming",
                gradient: "linear-gradient(125deg,#ffbe0b,#fb5607)",
            },
            {
                id: "openhardware",
                name: "Open Hardware",
                gradient: "linear-gradient(125deg,#06d6a0,#118ab2)",
            },
            {
                id: "connectivity",
                name: "Connectivity",
                gradient: "linear-gradient(125deg,#9d4edd,#ff70a6)",
            },
            {
                id: "robotics",
                name: "Robotics",
                gradient: "linear-gradient(125deg,#0077b6,#caf0f8)",
            },
            {
                id: "data",
                name: "Data",
                gradient: "linear-gradient(125deg,#606c38,#dda15e)",
            },
            {
                id: "ai",
                name: "AI",
                gradient: "linear-gradient(125deg,#f15bb5,#00bbf9)",
            },
        ],
        eu: [
            {
                id: "languages",
                name: "Languages",
                gradient: "linear-gradient(125deg,#ffafcc,#cdb4db)",
            },
            {
                id: "systems",
                name: "Systems",
                gradient: "linear-gradient(125deg,#023e8a,#48cae4)",
            },
            {
                id: "privacy",
                name: "Privacy",
                gradient: "linear-gradient(125deg,#e76f51,#e9c46a)",
            },
            {
                id: "opensource",
                name: "Open Source",
                gradient: "linear-gradient(125deg,#5e60ce,#80ffdb)",
            },
            {
                id: "standards",
                name: "Standards",
                gradient: "linear-gradient(125deg,#2d6a4f,#b7e4c7)",
            },
            {
                id: "processors",
                name: "Processors",
                gradient: "linear-gradient(125deg,#7b2cbf,#ff99c8)",
            },
        ],
        af: [
            {
                id: "connectivity",
                name: "Connectivity",
                gradient: "linear-gradient(125deg,#ffba08,#d00000)",
            },
            {
                id: "openhardware",
                name: "Open Hardware",
                gradient: "linear-gradient(125deg,#007f5f,#80b918)",
            },
            {
                id: "mobile",
                name: "Mobile",
                gradient: "linear-gradient(125deg,#e36414,#ffba08)",
            },
            {
                id: "infrastructure",
                name: "Infrastructure",
                gradient: "linear-gradient(125deg,#480ca8,#4cc9f0)",
            },
            {
                id: "ai",
                name: "AI",
                gradient: "linear-gradient(125deg,#6a994e,#f2e8cf)",
            },
            {
                id: "security",
                name: "Security",
                gradient: "linear-gradient(125deg,#f72585,#b5179e)",
            },
        ],
        asia: [
            {
                id: "processors",
                name: "Processors",
                gradient: "linear-gradient(125deg,#ff477e,#ffba08)",
            },
            {
                id: "robotics",
                name: "Robotics",
                gradient: "linear-gradient(125deg,#00b4d8,#03045e)",
            },
            {
                id: "ai",
                name: "AI",
                gradient: "linear-gradient(125deg,#8338ec,#ff006e)",
            },
            {
                id: "systems",
                name: "Systems",
                gradient: "linear-gradient(125deg,#3a86ff,#bde0fe)",
            },
            {
                id: "manufacturing",
                name: "Manufacturing",
                gradient: "linear-gradient(125deg,#386641,#f2e8cf)",
            },
            {
                id: "networks",
                name: "Networks",
                gradient: "linear-gradient(125deg,#560bad,#f15bb5)",
            },
        ],
        oc: [
            {
                id: "climatetech",
                name: "Climate Tech",
                gradient: "linear-gradient(125deg,#00f5d4,#00bbf9)",
            },
            {
                id: "networks",
                name: "Networks",
                gradient: "linear-gradient(125deg,#0077b6,#90e0ef)",
            },
            {
                id: "openhardware",
                name: "Open Hardware",
                gradient: "linear-gradient(125deg,#ff7b00,#ff006e)",
            },
            {
                id: "security",
                name: "Security",
                gradient: "linear-gradient(125deg,#4361ee,#4cc9f0)",
            },
            {
                id: "robotics",
                name: "Robotics",
                gradient: "linear-gradient(125deg,#588157,#dad7cd)",
            },
            {
                id: "ai",
                name: "AI",
                gradient: "linear-gradient(125deg,#b5179e,#f9c74f)",
            },
        ],
    };

    const storySeeds: Record<
        string,
        { title: string; excerpt: string; team: string }
    > = {
        languages: {
            title: "A closer look at memory-safe language design",
            excerpt:
                "A technical examination of type systems, ownership, compiler guarantees, and the trade-offs involved in building reliable software.",
            team: "Language Engineering",
        },
        systems: {
            title: "The next generation of operating systems",
            excerpt:
                "An investigation into kernel architecture, concurrency, isolation, and the engineering decisions behind more predictable computing systems.",
            team: "Systems Engineering",
        },
        hacks: {
            title: "Reverse engineering an unexpected performance bottleneck",
            excerpt:
                "A reproducible experiment investigates profiling results, implementation details, and an optimization that produces surprising results.",
            team: "Independent Research",
        },
        processors: {
            title: "RISC-V and the future of extensible processor design",
            excerpt:
                "Architecture specifications and implementation experiments reveal the trade-offs behind instruction sets, execution units, and silicon efficiency.",
            team: "Hardware Analysis",
        },
        security: {
            title: "Revisiting capability-based security boundaries",
            excerpt:
                "A technical review of privilege separation, isolation guarantees, attack surfaces, and the assumptions that determine system security.",
            team: "Security Research",
        },
        ai: {
            title: "Smaller inference engines challenge conventional runtimes",
            excerpt:
                "Engineers examine model execution, memory requirements, and optimized inference pipelines for constrained computing environments.",
            team: "Applied Computing",
        },
        silicon: {
            title: "Inside the engineering of next-generation silicon",
            excerpt:
                "An examination of fabrication constraints, packaging approaches, and the physical limitations that shape high-performance processor design.",
            team: "Silicon Research",
        },
        cybersecurity: {
            title: "Hardening software supply chains against new threats",
            excerpt:
                "Researchers explore reproducible builds, dependency integrity, and verification strategies for protecting critical software infrastructure.",
            team: "Security Research",
        },
        cloud: {
            title: "Reducing latency across distributed cloud infrastructure",
            excerpt:
                "A technical study of data locality, scheduling, networking, and resource isolation across large-scale computing environments.",
            team: "Infrastructure Engineering",
        },
        programming: {
            title: "The tools powering a new generation of developers",
            excerpt:
                "A regional engineering review explores accessible development environments, compiler tooling, and projects expanding open computing.",
            team: "Developer Research",
        },
        openhardware: {
            title: "Open hardware designs move beyond the prototype stage",
            excerpt:
                "Independent builders examine board layouts, embedded firmware, documentation, and manufacturing constraints in community-driven hardware.",
            team: "Open Hardware Research",
        },
        connectivity: {
            title: "Building resilient networks beyond major urban centres",
            excerpt:
                "An investigation into network architecture, low-cost infrastructure, and practical approaches to maintaining reliable connections.",
            team: "Network Engineering",
        },
        robotics: {
            title: "Robotics research tackles real-world control problems",
            excerpt:
                "Engineers investigate control loops, embedded computation, sensor fusion, and the reliability challenges of autonomous machines.",
            team: "Robotics Research",
        },
        data: {
            title: "New approaches to dependable data infrastructure",
            excerpt:
                "A study of distributed storage, integrity checks, data locality, and the practical constraints of maintaining dependable systems.",
            team: "Data Engineering",
        },
        privacy: {
            title: "Privacy engineering moves closer to the operating system",
            excerpt:
                "A technical investigation explores data minimization, local processing, application permissions, and auditable privacy boundaries.",
            team: "Privacy Research",
        },
        opensource: {
            title: "Maintaining critical open-source infrastructure",
            excerpt:
                "A look at long-term maintenance, contributor coordination, reproducible releases, and engineering practices for dependable public software.",
            team: "Open Source Research",
        },
        standards: {
            title: "Interoperability standards face increasingly complex systems",
            excerpt:
                "Engineers examine specifications, conformance tests, compatibility constraints, and the work required to make implementations agree.",
            team: "Standards Research",
        },
        mobile: {
            title: "Efficient mobile computing beyond flagship hardware",
            excerpt:
                "A technical look at power management, offline-first software, memory constraints, and dependable applications on lower-power devices.",
            team: "Mobile Engineering",
        },
        infrastructure: {
            title: "Designing dependable infrastructure under real constraints",
            excerpt:
                "A systems-level investigation of power, networks, hardware availability, and the trade-offs involved in maintaining reliable services.",
            team: "Infrastructure Research",
        },
        manufacturing: {
            title: "Automation changes the economics of hardware manufacturing",
            excerpt:
                "An engineering analysis explores robotics, inspection systems, fabrication yield, and the interaction between software and production.",
            team: "Manufacturing Research",
        },
        networks: {
            title: "New network architectures target difficult performance limits",
            excerpt:
                "A closer look at routing, transport protocols, congestion control, and implementation decisions that influence network performance.",
            team: "Network Research",
        },
        climatetech: {
            title: "Engineering more efficient climate-monitoring systems",
            excerpt:
                "Researchers investigate sensor networks, low-power computing, data quality, and the infrastructure needed for environmental monitoring.",
            team: "Climate Technology Research",
        },
    };

    const regionalAuthors: Record<string, string[]> = {
        global: ["John Doe", "Dave Wilson"],
        na: ["Maya Chen", "Theo Park"],
        sa: ["Lucia Costa", "Rafael Silva"],
        eu: ["Elena Fischer", "Alex Morgan"],
        af: ["Amara Okafor", "Kofi Mensah"],
        asia: ["Priya Nair", "Kenji Sato"],
        oc: ["Jordan Blake", "Isla Bennett"],
    };

    const adminEmails = ["admin@foews.example", "maya.chen@example.com"];
    const factCheckerEmails = [
        "factcheck@foews.example",
        "elena.fischer@example.com",
    ];
    const blacklistedEmails = [
        "banned@foews.example",
        "restricted@foews.example",
    ];
    const MAX_POST_STORAGE_BYTES = 10 * 1024 * 1024;
    const factCheckerRoster = ["Elena Fischer", "Foews Fact-Check Team"];
    const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    const namePattern = /^[\x20-\x7E]+$/;
    const passwordRules = [
        {
            label: "At least 12 characters",
            test: (value: string) => value.length >= 12,
        },
        {
            label: "One uppercase letter",
            test: (value: string) => /[A-Z]/.test(value),
        },
        {
            label: "One lowercase letter",
            test: (value: string) => /[a-z]/.test(value),
        },
        { label: "One number", test: (value: string) => /\d/.test(value) },
        {
            label: "One symbol",
            test: (value: string) => /[^A-Za-z0-9]/.test(value),
        },
    ];

    const initialSources: Record<string, string[]> = {
        languages: [
            "https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html",
            "https://doc.rust-lang.org/reference/type-system.html",
            "https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines",
        ],
        systems: [
            "https://docs.sel4.systems/",
            "https://docs.kernel.org/",
            "https://pubs.opengroup.org/onlinepubs/9799919799/",
        ],
        programming: [
            "https://docs.python.org/3/",
            "https://code.visualstudio.com/docs",
            "https://gcc.gnu.org/onlinedocs/",
        ],
        openhardware: [
            "https://docs.arduino.cc/",
            "https://docs.kicad.org/",
            "https://www.oshwa.org/definition/",
        ],
        connectivity: [
            "https://www.rfc-editor.org/rfc/rfc8200",
            "https://www.rfc-editor.org/rfc/rfc9000",
            "https://www.internetsociety.org/issues/community-networks/",
        ],
        processors: [
            "https://riscv.org/",
            "https://docs.riscv.org/",
            "https://riscv.github.io/riscv-isa-manual/snapshot/unprivileged/",
        ],
        robotics: [
            "https://docs.ros.org/",
            "https://control.ros.org/",
            "https://gazebosim.org/docs/",
        ],
        climatetech: [
            "https://gml.noaa.gov/ccgg/trends/",
            "https://science.nasa.gov/climate-change/",
            "https://wmo.int/",
        ],
        networks: [
            "https://www.rfc-editor.org/rfc/rfc9000",
            "https://www.rfc-editor.org/rfc/rfc9293",
            "https://www.rfc-editor.org/rfc/rfc9438",
        ],
    };

    const originalSources = [
        [
            "https://sel4.systems/",
            "https://docs.sel4.systems/",
            "https://docs.kernel.org/",
        ],
        [
            "https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines",
            "https://doc.rust-lang.org/book/",
            "https://docs.kernel.org/dev-tools/testing-overview.html",
        ],
        [
            "https://www.spec.org/cpu2017/Docs/",
            "https://github.com/google/benchmark",
            "https://llvm.org/docs/Benchmarking.html",
        ],
        [
            "https://docs.riscv.org/",
            "https://gcc.gnu.org/onlinedocs/",
            "https://docs.kernel.org/",
        ],
        [
            "https://www.bestpractices.dev/en",
            "https://securityscorecards.dev/",
            "https://openssf.org/",
        ],
    ];

    function buildOriginals(): Article[] {
        const originalStories = [
            {
                title: "Inside the Kernel: What Microkernel Engineering Actually Costs",
                excerpt:
                    "A Foews investigation into the architectural trade-offs behind small kernels, formal verification, device drivers, and dependable system boundaries.",
                body: [
                    "Microkernels are frequently presented as a clean architectural answer to a complicated engineering problem. Their appeal is understandable: move services out of privileged execution, reduce the trusted computing base, and create boundaries that can be reasoned about independently.",
                    "The practical cost is less glamorous. Interprocess communication, service restart semantics, capability management, driver isolation, and debugging across address-space boundaries all introduce design work. Optimizing one path can make another less predictable, while an abstraction that simplifies the kernel may complicate the surrounding system.",
                    "A serious comparison therefore has to examine more than kernel size or a single benchmark. It needs to document what runs in user space, how failures propagate, how drivers are maintained, and which security guarantees are enforced by design rather than policy.",
                    "The most useful question is not whether microkernels are universally superior. It is which workloads and threat models justify their additional engineering costs, and whether the implementation provides the evidence needed to support those claims.",
                ],
            },
            {
                title: "Designing Reliable Software Under Severe Constraints",
                excerpt:
                    "An editorial analysis of memory budgets, testing discipline, long-term maintenance, and the decisions behind dependable software.",
                body: [
                    "Software built for strict memory and latency limits benefits from measurable budgets. Teams need to know what they can spend on allocation, scheduling, communication, and recovery before implementation choices become difficult to change.",
                    "Clear invariants and focused tests make low-level systems easier to maintain as they grow. The important boundaries should be explicit enough that engineers can review the reasoning without relying on one person's memory.",
                    "Performance claims should remain reproducible. Workload, configuration, compiler, hardware, and measurement method all affect a result and belong alongside the reported numbers.",
                    "Reliable software depends on constraints, invariants, and claims that can be checked. Documenting those decisions throughout a system's life helps engineering teams make trade-offs they can explain and revisit.",
                ],
            },
            {
                title: "The Benchmark Trap: When Faster Numbers Mislead Engineers",
                excerpt:
                    "An original editorial investigation into benchmark selection, compiler flags, measurement noise, and the temptation to turn a narrow performance result into a sweeping claim.",
                body: [
                    "Benchmarks are essential tools, but they are not neutral summaries of reality. Workload selection, warm-up behaviour, CPU frequency, cache state, compiler configuration, and measurement overhead can materially change the result.",
                    "A convincing report documents the environment and explains why the workload represents the problem under discussion. It also reports limitations and investigates outliers instead of removing inconvenient measurements without justification.",
                    "Comparisons become especially fragile when one implementation receives careful tuning while another is measured with defaults. A meaningful experiment gives each candidate comparable treatment and makes the methodology available for independent reproduction.",
                    "Foews recommends treating every performance headline as the beginning of an investigation. The benchmark is evidence, not the conclusion.",
                ],
            },
            {
                title: "Beyond the Specification: How Hardware Features Reach Real Software",
                excerpt:
                    "An original report examining the chain from instruction-set specifications to compiler support, kernel integration, diagnostics, and applications that actually benefit.",
                body: [
                    "A hardware feature can be correctly specified, implemented in silicon, and still provide little practical benefit to most software. The feature must pass through several independent layers before application developers can rely on it.",
                    "Compilers need appropriate code generation and feature detection. Operating systems need to manage capabilities and expose reliable interfaces. Libraries need usable abstractions, and applications need workloads that benefit enough to justify the implementation complexity.",
                    "The result is a coordination problem as much as a hardware problem. A successful launch depends on documentation, testing, toolchain maturity, and compatibility across the environments developers actually use.",
                    "Evaluating a new feature therefore requires evidence from the whole stack rather than an isolated specification or theoretical peak-performance figure.",
                ],
            },
            {
                title: "The Maintenance Debt Behind Critical Open-Source Infrastructure",
                excerpt:
                    "A Foews editorial investigation into maintainer capacity, release processes, security response, and the hidden engineering work that keeps foundational software dependable.",
                body: [
                    "Critical open-source infrastructure depends on people who review patches, handle regressions, investigate security reports, maintain build systems, and preserve compatibility over time. These responsibilities continue long after the initial implementation is celebrated.",
                    "The health of a project is difficult to measure through commit counts alone. Review queues, release discipline, contributor concentration, test coverage, and the ability to respond to vulnerabilities reveal much more about operational resilience.",
                    "Sustainable maintenance requires funding, clear ownership, manageable release practices, and ways for new contributors to learn the codebase. Without those foundations, even technically excellent software can accumulate risks that are difficult to see from the outside.",
                    "This investigation argues for treating maintenance capacity as an engineering property rather than an administrative detail.",
                ],
            },
        ];
        const authors = [
            "Maya Chen",
            "Foews Editorial",
            "Elena Fischer",
            "Theo Park",
            "Jordan Blake",
        ];
        const published = [
            "Today",
            "Yesterday",
            "2 days ago",
            "4 days ago",
            "1 week ago",
        ];
        const readTimes = [
            "12 min read",
            "9 min read",
            "11 min read",
            "10 min read",
            "13 min read",
        ];
        const gradients = [
            "linear-gradient(125deg,#3a0ca3,#4cc9f0)",
            "linear-gradient(125deg,#f3722c,#f9c74f)",
            "linear-gradient(125deg,#43aa8b,#90be6d)",
            "linear-gradient(125deg,#7209b7,#f72585)",
            "linear-gradient(125deg,#00b4d8,#4361ee)",
        ];
        return originalStories.map((story, index) => ({
            id: 5000 + index,
            title: story.title,
            excerpt: story.excerpt,
            body: story.body,
            region: "originals",
            topic: "originals",
            topicLabel: "Foews Originals",
            author: authors[index],
            team: "Foews Editorial",
            published: published[index],
            readTime: readTimes[index],
            likes: 0,
            liked: false,
            verified: true,
            verifier: "Foews Fact-Check Team",
            verifiedAt: "25 Oct 2026, 12:42 UTC",
            sources: originalSources[index],
            gradient: gradients[index],
        }));
    }

    const profileSeeds: Article[] = [
        {
            id: 9001,
            title: "Tracing a concurrency bug through the scheduler",
            excerpt:
                "A technical post following a scheduling issue through a reproducible test and a detailed investigation of thread interactions.",
            body: [
                "A concurrency failure can appear only under a particular scheduling interleaving, making it difficult to reproduce with ordinary test runs.",
                "A useful investigation starts by reducing the failing case, recording the relevant runtime conditions, and identifying the invariants the scheduler is expected to preserve.",
                "Reducing the failing case and recording the runtime conditions makes the scheduler's expected invariants easier to inspect.",
            ],
            region: "account",
            topic: "systems",
            topicLabel: "Systems",
            author: "Maya Chen",
            team: "Personal Posts",
            published: "Yesterday",
            readTime: "7 min read",
            likes: 0,
            liked: false,
            verified: false,
            verifier: "",
            verifiedAt: "",
            sources: [],
            gradient: "linear-gradient(125deg,#00b4d8,#4361ee)",
        },
        {
            id: 9002,
            title: "A practical experiment with compiler diagnostics",
            excerpt:
                "An engineering note exploring clearer diagnostics, smaller test cases, and a repeatable workflow for locating compiler errors.",
            body: [
                "Compiler diagnostics are most useful when they identify the violated rule and explain how the programmer reached the problematic state.",
                "Small reproductions make this process easier. They reduce unrelated complexity, shorten test cycles, and help identify which assumptions are essential to the failure.",
                "Focused examples help engineers compare diagnostic quality across compilers and language versions.",
            ],
            region: "account",
            topic: "languages",
            topicLabel: "Languages",
            author: "Maya Chen",
            team: "Personal Posts",
            published: "3 days ago",
            readTime: "5 min read",
            likes: 0,
            liked: false,
            verified: false,
            verifier: "",
            verifiedAt: "",
            sources: [],
            gradient: "linear-gradient(125deg,#f9c74f,#f941a5)",
        },
        {
            id: 9003,
            title: "Notes on predictable memory allocation",
            excerpt:
                "An engineering note examining allocation patterns, fragmentation, and strategies for making memory usage easier to reason about.",
            body: [
                "Predictable memory use depends on more than choosing an allocator. Lifetime management, allocation patterns, fragmentation, and concurrent access all contribute to behaviour under load.",
                "A useful experiment measures steady-state and worst-case behaviour separately, while documenting the workload and configuration.",
                "Clear measurements help teams compare allocation strategies under realistic workloads.",
            ],
            region: "account",
            topic: "systems",
            topicLabel: "Systems",
            author: "Maya Chen",
            team: "Personal Posts",
            published: "1 week ago",
            readTime: "9 min read",
            likes: 0,
            liked: false,
            verified: false,
            verifier: "",
            verifiedAt: "",
            sources: [],
            gradient: "linear-gradient(125deg,#43aa8b,#90be6d)",
        },
    ];

    function buildArticles(): Article[] {
        const result: Article[] = [];
        const regionIds = ["global", "na", "sa", "eu", "af", "asia", "oc"];
        let id = 100;
        for (const regionId of regionIds) {
            const topics = topicCatalog[regionId];
            for (let index = 0; index < topics.length; index++) {
                const topic = topics[index];
                const story = storySeeds[topic.id];
                const articleId = id++;
                const verified = index < 2;
                const title = `${story.title}${regionId === "global" ? "" : ` in ${regionNames[regionId]}`}`;
                result.push({
                    id: articleId,
                    title,
                    excerpt: story.excerpt,
                    body: [
                        story.excerpt,
                        `The central question is which guarantees an implementation actually provides. The engineering choices are considered through the perspective of ${regionNames[regionId]}.`,
                        "Reproducibility remains essential. Reports should identify the hardware, software versions, configuration, and methodology used to obtain a result.",
                        "Implementation quality, maintenance costs, compatibility, and real engineering constraints all shape how useful a technical approach will be.",
                    ],
                    region: regionId,
                    topic: topic.id,
                    topicLabel: topic.name,
                    author: regionalAuthors[regionId][index % 2],
                    team: story.team,
                    published: `${12 + index * 8} minutes ago`,
                    readTime: `${4 + ((articleId * 7) % 8)} min read`,
                    likes: 20 + ((articleId * 47) % 371),
                    liked: false,
                    verified,
                    verifier: verified ? "Foews Fact-Check Team" : "",
                    verifiedAt: verified ? "25 Oct 2026, 12:42 UTC" : "",
                    sources: verified
                        ? [
                              ...(initialSources[topic.id] ??
                                  initialSources.systems),
                          ]
                        : [],
                    gradient: topic.gradient,
                });
            }
        }
        return result;
    }

    function loadSavedProfile(): {
        name?: string;
        email?: string;
        phoneCountry?: string;
        phoneNumber?: string;
    } {
        if (typeof localStorage === "undefined") return {};
        try {
            return JSON.parse(localStorage.getItem("foews-profile") ?? "{}");
        } catch {
            return {};
        }
    }
    const savedProfile = loadSavedProfile();
    const accountName = savedProfile.name ?? "Maya Chen";
    const accountEmail = savedProfile.email ?? "maya.chen@example.com";
    let profileName = $state(savedProfile.name ?? "Maya Chen");
    let profileEmail = $state(savedProfile.email ?? "maya.chen@example.com");
    let phoneCountry = $state(savedProfile.phoneCountry ?? "");
    let phoneNumber = $state(savedProfile.phoneNumber ?? "");
    let articles = $state<Article[]>([
        ...buildArticles(),
        ...buildOriginals(),
        ...profileSeeds.map((article) => ({
            ...article,
            author: savedProfile.name ?? article.author,
        })),
    ]);
    let selectedRegion = $state("global");
    let activeTopic = $state("world");
    let selectedWriter = $state<Writer | null>(null);
    let writerFirstName = $state("");
    let writerLastName = $state("");
    let openArticleId = $state<number | null>(null);
    let darkMode = $state(false);
    let showExcerpts = $state(true);
    let compactMode = $state(false);
    let subscriptionActive = $state(false);
    let subscriptionMessage = $state("");
    let subscriptionMessageError = $state(false);
    let billingAttempted = $state(false);
    let billingName = $state(savedProfile.name ?? "Maya Chen");
    let billingEmail = $state(savedProfile.email ?? "maya.chen@example.com");
    let cardNumber = $state("");
    let cardExpiry = $state("");
    let cardCvc = $state("");
    let accountSaved = $state(false);
    let accountAttempted = $state(false);
    let passwordEditing = $state(false);
    let currentPassword = $state("");
    let newPassword = $state("");
    let passwordMessage = $state("");
    let passwordAttempted = $state(false);
    let showCurrentPassword = $state(false);
    let showNewPassword = $state(false);
    let accountMessage = $state("");
    let reportArticle = $state<number | null>(null);
    let reportText = $state("");
    let reportMessage = $state("");
    let reportSubmitted = $state(false);
    let factCheckAttempted = $state(false);
    let factFileError = $state("");
    let postFileError = $state("");
    let postFileWarning = $state("");
    let downloadError = $state("");
    let showBlacklistMessage = $state(false);
    let regionMenuOpen = $state(false);
    let topicMenuOpen = $state(false);
    let verifierMenuOpen = $state(false);
    let factCheckArticle = $state<number | null>(null);
    let factCheckSources = $state(["", "", ""]);
    let factCheckFiles = $state<ArticleAttachment[]>([]);
    let factCheckMessage = $state("");
    let factCheckSubmitted = $state(false);
    let factCheckedTitle = $state("");
    let toastMessage = $state("");
    let createTitle = $state("");
    let createExcerpt = $state("");
    let createBody = $state("");
    let createLinks = $state(["", "", ""]);
    let createFiles = $state<ArticleAttachment[]>([]);
    let createLinksAttempted = $state(false);
    let createRegion = $state("global");
    let createTopic = $state("languages");
    let createOriginal = $state(false);
    let createAuthor = $state("Maya Chen");
    let createVerifier = $state("Elena Fischer");
    let createOriginalSources = $state(["", "", ""]);
    let createOriginalSourcesAttempted = $state(false);
    let createError = $state("");
    let swipeGesture: {
        id: number;
        x: number;
        y: number;
        pointerId: number;
    } | null = null;
    let suppressedArticleClick: number | null = null;
    let activeSwipeId = $state<number | null>(null);

    const normalizedEmail = accountEmail.trim().toLowerCase();
    const isBlacklisted = $derived(
        blacklistedEmails
            .map((email) => email.toLowerCase())
            .includes(normalizedEmail),
    );
    const isAdmin = $derived(
        !isBlacklisted &&
            adminEmails
                .map((email) => email.toLowerCase())
                .includes(normalizedEmail),
    );
    const isFactChecker = $derived(
        !isBlacklisted &&
            factCheckerEmails
                .map((email) => email.toLowerCase())
                .includes(normalizedEmail),
    );
    const writerCollator = new Intl.Collator("en", {
        sensitivity: "base",
        numeric: true,
    });
    const writers = $derived.by(() => {
        const directory = new Map<string, Writer>();
        for (const article of articles) {
            if (article.deleted) continue;
            const name = article.author.trim().replace(/\s+/g, " ");
            if (!name) continue;
            const id = writerKey(name);
            if (directory.has(id)) continue;
            const [firstName = name, ...lastName] = name.split(" ");
            directory.set(id, {
                id,
                name,
                firstName,
                lastName: lastName.join(" "),
                gradient: article.gradient,
            });
        }
        return [...directory.values()].sort(
            (a, b) =>
                writerCollator.compare(a.firstName, b.firstName) ||
                writerCollator.compare(a.lastName, b.lastName) ||
                writerCollator.compare(a.name, b.name),
        );
    });
    const filteredWriters = $derived.by(() => {
        const first = normalizeWriterSearch(writerFirstName);
        const last = normalizeWriterSearch(writerLastName);
        return writers.filter(
            (writer) =>
                normalizeWriterSearch(writer.firstName).startsWith(first) &&
                (!last ||
                    normalizeWriterSearch(writer.lastName).startsWith(last)),
        );
    });
    const writerGroups = $derived.by(() => {
        const groups = new Map<string, Writer[]>();
        for (const writer of filteredWriters) {
            const initial =
                [...normalizeWriterSearch(writer.firstName)][0] ?? "#";
            const letter = /^\p{L}$/u.test(initial)
                ? initial.toLocaleUpperCase("en")
                : "#";
            const group = groups.get(letter) ?? [];
            group.push(writer);
            groups.set(letter, group);
        }
        return [...groups].map(([letter, members]) => ({ letter, members }));
    });
    const writerArticles = $derived.by(() => {
        const id = selectedWriter?.id;
        return id
            ? articles.filter(
                  (article) =>
                      !article.deleted && writerKey(article.author) === id,
              )
            : [];
    });
    const readableWriterArticles = $derived(
        writerArticles.filter(
            (article) =>
                article.region !== "originals" || subscriptionActive || isAdmin,
        ),
    );
    const lockedWriterOriginals = $derived(
        writerArticles.filter(
            (article) =>
                article.region === "originals" &&
                !subscriptionActive &&
                !isAdmin,
        ).length,
    );
    const writerTopics = $derived.by(() => {
        const topics = new Map<string, Topic>();
        for (const article of readableWriterArticles) {
            if (!topics.has(article.topic))
                topics.set(article.topic, {
                    id: article.topic,
                    name: article.topicLabel,
                    gradient: article.gradient,
                });
        }
        return [...topics.values()];
    });
    const currentTopics = $derived(
        selectedRegion === "writers" && selectedWriter
            ? writerTopics
            : (topicCatalog[selectedRegion] ?? topicCatalog.global),
    );
    const currentHello = $derived(
        selectedRegion === "writers" && selectedWriter
            ? helloSnippets.eu
            : (helloSnippets[selectedRegion] ?? helloSnippets.global),
    );
    const currentSlogan = $derived(
        regionSlogans[selectedRegion] ?? regionSlogans.global,
    );
    const selectedArticle = $derived(
        articles.find(
            (article) => article.id === openArticleId && !article.deleted,
        ) ?? null,
    );
    const deletedArticles = $derived(
        articles.filter(
            (article) =>
                article.deleted && (isAdmin || article.author === accountName),
        ),
    );
    const relatedArticles = $derived.by(() => {
        if (!selectedArticle) return [];
        const available =
            selectedArticle.region === "originals"
                ? articles.filter(
                      (article) =>
                          article.region === "originals" &&
                          article.id !== selectedArticle.id &&
                          !article.deleted,
                  )
                : selectedArticle.region === "account"
                  ? articles.filter(
                        (article) =>
                            article.region === "account" &&
                            article.id !== selectedArticle.id &&
                            !article.deleted,
                    )
                  : articles.filter(
                        (article) =>
                            article.id !== selectedArticle.id &&
                            article.region !== "originals" &&
                            !article.deleted &&
                            (article.topic === selectedArticle.topic ||
                                article.region === selectedArticle.region),
                    );
        return available.slice(0, 3);
    });
    const profilePosts = $derived(
        articles
            .filter(
                (article) =>
                    article.author === accountName &&
                    article.region !== "originals" &&
                    article.region !== "account" &&
                    !article.deleted,
            )
            .concat(
                articles.filter(
                    (article) =>
                        article.author === accountName &&
                        article.region === "account" &&
                        !article.deleted,
                ),
            ),
    );
    const visibleArticles = $derived.by(() => {
        let list = articles.filter((article) => !article.deleted);
        if (selectedRegion === "writers") {
            list = selectedWriter ? readableWriterArticles : [];
        } else if (selectedRegion === "originals") {
            if (!subscriptionActive && !isAdmin) return [];
            list = list.filter((article) => article.region === "originals");
        } else {
            list = list.filter((article) => article.region === selectedRegion);
        }
        if (activeTopic !== "world" && selectedRegion !== "originals")
            list = list.filter((article) => article.topic === activeTopic);
        return list;
    });
    const relatedTopics = $derived(
        (selectedRegion === "originals" ||
        (selectedRegion === "writers" && !currentTopics.length)
            ? topicCatalog.global
            : currentTopics
        )
            .filter((topic) => topic.id !== activeTopic)
            .slice(0, 3),
    );
    const createTopics = $derived(
        topicCatalog[createRegion] ?? topicCatalog.global,
    );
    const currentTopic = $derived(
        createTopics.find((topic) => topic.id === createTopic) ??
            createTopics[0],
    );
    const duplicateCreateLinks = $derived(
        createLinks
            .map(normalizeLink)
            .filter(Boolean)
            .filter((link, index, all) => all.indexOf(link) !== index),
    );
    const duplicateFactLinks = $derived(
        factCheckSources
            .map(normalizeLink)
            .filter(Boolean)
            .filter((link, index, all) => all.indexOf(link) !== index),
    );
    const duplicateOriginalSources = $derived(
        createOriginalSources
            .map(normalizeLink)
            .filter(Boolean)
            .filter((link, index, all) => all.indexOf(link) !== index),
    );
    const phoneSet = $derived(
        phoneCountry.trim() !== "" && phoneNumber.trim() !== "",
    );
    const validCallingCodes = new Set([
        "1",
        "7",
        "20",
        "27",
        "30",
        "31",
        "32",
        "33",
        "34",
        "36",
        "39",
        "40",
        "41",
        "43",
        "44",
        "45",
        "46",
        "47",
        "48",
        "49",
        "51",
        "52",
        "53",
        "54",
        "55",
        "56",
        "57",
        "58",
        "60",
        "61",
        "62",
        "63",
        "64",
        "65",
        "66",
        "81",
        "82",
        "84",
        "86",
        "90",
        "91",
        "92",
        "93",
        "94",
        "95",
        "98",
        "211",
        "212",
        "213",
        "216",
        "218",
        "220",
        "221",
        "222",
        "223",
        "224",
        "225",
        "226",
        "227",
        "228",
        "229",
        "230",
        "231",
        "232",
        "233",
        "234",
        "235",
        "236",
        "237",
        "238",
        "239",
        "240",
        "241",
        "242",
        "243",
        "244",
        "245",
        "246",
        "248",
        "249",
        "250",
        "251",
        "252",
        "253",
        "254",
        "255",
        "256",
        "257",
        "258",
        "260",
        "261",
        "262",
        "263",
        "264",
        "265",
        "266",
        "267",
        "268",
        "269",
        "290",
        "291",
        "297",
        "298",
        "299",
        "350",
        "351",
        "352",
        "353",
        "354",
        "355",
        "356",
        "357",
        "358",
        "359",
        "370",
        "371",
        "372",
        "373",
        "374",
        "375",
        "376",
        "377",
        "378",
        "380",
        "381",
        "382",
        "383",
        "385",
        "386",
        "387",
        "389",
        "420",
        "421",
        "423",
        "500",
        "501",
        "502",
        "503",
        "504",
        "505",
        "506",
        "507",
        "508",
        "509",
        "590",
        "591",
        "592",
        "593",
        "594",
        "595",
        "596",
        "597",
        "598",
        "599",
        "670",
        "672",
        "673",
        "674",
        "675",
        "676",
        "677",
        "678",
        "679",
        "680",
        "681",
        "682",
        "683",
        "685",
        "686",
        "687",
        "688",
        "689",
        "690",
        "691",
        "692",
        "850",
        "852",
        "853",
        "855",
        "856",
        "880",
        "886",
        "960",
        "961",
        "962",
        "963",
        "964",
        "965",
        "966",
        "967",
        "968",
        "970",
        "971",
        "972",
        "973",
        "974",
        "975",
        "976",
        "977",
        "992",
        "993",
        "994",
        "995",
        "996",
        "998",
    ]);
    const countryValid = $derived(
        phoneCountry.trim().startsWith("+") &&
            validCallingCodes.has(phoneCountry.trim().slice(1)),
    );
    const phoneValid = $derived(
        /^[0-9][0-9\s().-]{5,17}$/.test(phoneNumber.trim()) &&
            /\d{6,15}/.test(phoneNumber.replace(/\D/g, "")),
    );
    const phoneWarning = $derived(!phoneCountry.trim() && !phoneNumber.trim());
    const passwordValid = $derived(
        passwordRules.every((rule) => rule.test(newPassword)),
    );
    const createTotalFileBytes = $derived(
        createFiles.reduce((total, attachment) => total + attachment.size, 0),
    );

    $effect(() => {
        if (typeof document !== "undefined")
            document.documentElement.dataset.theme = darkMode
                ? "dark"
                : "light";
    });
    $effect(() => {
        if (createRegion === "originals") createRegion = "global";
        if (!createTopics.some((topic) => topic.id === createTopic))
            createTopic = createTopics[0]?.id ?? "languages";
    });
    $effect(() => {
        if (
            selectedRegion === "writers" &&
            selectedWriter &&
            activeTopic !== "world" &&
            !writerTopics.some((topic) => topic.id === activeTopic)
        )
            activeTopic = "world";
    });

    function writerKey(name: string): string {
        return name.trim().replace(/\s+/g, " ").toLocaleLowerCase("en");
    }

    function normalizeWriterSearch(value: string): string {
        return value
            .normalize("NFKD")
            .replace(/\p{M}/gu, "")
            .trim()
            .replace(/\s+/g, " ")
            .toLocaleLowerCase("en");
    }

    function selectWriter(id: string) {
        const writer = writers.find((item) => item.id === id);
        if (!writer) return;
        selectedWriter = { ...writer };
        selectedRegion = "writers";
        activeTopic = "world";
        openArticleId = null;
        activeSwipeId = null;
        scrollTop();
    }

    function scrollTop() {
        window.scrollTo({ top: 0, behavior: "smooth" });
    }
    function clearBillingMessage() {
        subscriptionMessage = "";
        subscriptionMessageError = false;
    }

    function verifierDropdown(node: HTMLElement, open: boolean) {
        const focusSelected = (expanded: boolean) => {
            if (expanded)
                queueMicrotask(() => {
                    if (node.isConnected)
                        node
                            .querySelector<HTMLButtonElement>(
                                '[role="option"][aria-selected="true"]',
                            )
                            ?.focus();
                });
        };
        const closeOutside = (event: PointerEvent) => {
            if (!node.contains(event.target as Node)) verifierMenuOpen = false;
        };
        document.addEventListener("pointerdown", closeOutside);
        focusSelected(open);
        return {
            update: focusSelected,
            destroy() {
                document.removeEventListener("pointerdown", closeOutside);
                verifierMenuOpen = false;
            },
        };
    }

    function verifierKeydown(event: KeyboardEvent) {
        const target = event.currentTarget as HTMLButtonElement;
        const dropdown = target.closest(".custom-select");
        if (!dropdown) return;
        if (event.key === "Escape") {
            event.preventDefault();
            verifierMenuOpen = false;
            dropdown
                .querySelector<HTMLButtonElement>(".select-trigger")
                ?.focus();
        } else if (event.key === "Tab") {
            verifierMenuOpen = false;
        } else if (
            ["ArrowDown", "ArrowUp", "Home", "End"].includes(event.key)
        ) {
            event.preventDefault();
            if (!verifierMenuOpen) {
                verifierMenuOpen = true;
                regionMenuOpen = false;
                topicMenuOpen = false;
                return;
            }
            const options = Array.from(
                dropdown.querySelectorAll<HTMLButtonElement>('[role="option"]'),
            );
            const index = options.indexOf(target);
            const next =
                event.key === "Home"
                    ? 0
                    : event.key === "End"
                      ? options.length - 1
                      : (index +
                            (event.key === "ArrowDown" ? 1 : -1) +
                            options.length) %
                        options.length;
            options[next]?.focus();
        }
    }

    function selectRegion(id: string) {
        selectedWriter = null;
        if (id === "create") {
            openArticleId = null;
            activeSwipeId = null;
            selectedRegion = "create";
            createError = "";
            scrollTop();
            return;
        }
        selectedRegion = id;
        activeTopic = "world";
        openArticleId = null;
        subscriptionMessage = "";
        subscriptionMessageError = false;
        reportArticle = null;
        factCheckArticle = null;
        activeSwipeId = null;
        scrollTop();
    }
    function selectTopic(id: string) {
        if (
            ["account", "settings", "originals", "create"].includes(
                selectedRegion,
            ) ||
            (selectedRegion === "writers" &&
                (!selectedWriter ||
                    !writerTopics.some((topic) => topic.id === id)))
        ) {
            selectedRegion = "global";
            selectedWriter = null;
        }
        activeTopic = id;
        openArticleId = null;
        activeSwipeId = null;
        scrollTop();
    }
    function openArticle(id: number) {
        const article = articles.find(
            (item) => item.id === id && !item.deleted,
        );
        if (!article) return;
        if (article.region === "originals" && !subscriptionActive && !isAdmin) {
            selectRegion("originals");
            return;
        }
        openArticleId = id;
        activeSwipeId = null;
        scrollTop();
    }
    function closeArticle() {
        openArticleId = null;
        activeSwipeId = null;
        scrollTop();
    }
    function toggleLike(id: number) {
        if (isBlacklisted) return;
        articles = articles.map((article) =>
            article.id === id && !article.deleted
                ? {
                      ...article,
                      liked: !article.liked,
                      likes: article.likes + (article.liked ? -1 : 1),
                  }
                : article,
        );
    }
    function formatLikes(count: number) {
        return new Intl.NumberFormat("en", {
            notation: count >= 10000 ? "compact" : "standard",
            maximumFractionDigits: 1,
        }).format(count);
    }
    function positionTooltip(event: Event) {
        const trigger = event.currentTarget as HTMLElement;
        const wrapper = trigger.parentElement;
        if (!wrapper) return;
        const bounds = trigger.getBoundingClientRect();
        const tooltip = wrapper.querySelector<HTMLElement>(
            ".verification-tooltip",
        );
        const width = Math.min(225, window.innerWidth - 20);
        const height = Math.min(
            tooltip?.offsetHeight ?? 180,
            window.innerHeight - 20,
        );
        const left = Math.max(
            10,
            Math.min(bounds.right - width, window.innerWidth - width - 10),
        );
        const above = bounds.top - height - 10;
        const top =
            above >= 10
                ? above
                : Math.max(
                      10,
                      Math.min(
                          bounds.bottom + 10,
                          window.innerHeight - height - 10,
                      ),
                  );
        wrapper.style.setProperty("--tooltip-left", `${left}px`);
        wrapper.style.setProperty("--tooltip-top", `${top}px`);
    }

    function canReport(article: Article) {
        return (
            !isBlacklisted && !article.deleted && article.author !== accountName
        );
    }

    function canFactCheck(article: Article) {
        return (
            !isBlacklisted &&
            isFactChecker &&
            !article.deleted &&
            !article.verified &&
            article.author !== accountName
        );
    }

    function canDelete(article: Article) {
        return (
            !isBlacklisted &&
            !article.deleted &&
            (isAdmin || article.author === accountName)
        );
    }

    function hasArticleActions(article: Article) {
        return (
            canReport(article) || canFactCheck(article) || canDelete(article)
        );
    }

    function toggleArticleActions(id: number) {
        const article = articles.find((item) => item.id === id);
        if (!article || !hasArticleActions(article)) return;
        activeSwipeId = activeSwipeId === id ? null : id;
    }

    function beginSwipe(event: PointerEvent, id: number) {
        const article = articles.find((item) => item.id === id);
        if (!article || !hasArticleActions(article) || event.button !== 0)
            return;
        swipeGesture = {
            id,
            x: event.clientX,
            y: event.clientY,
            pointerId: event.pointerId,
        };
        (event.currentTarget as HTMLButtonElement).setPointerCapture(
            event.pointerId,
        );
    }

    function endSwipe(event: PointerEvent, id: number) {
        if (
            !swipeGesture ||
            swipeGesture.id !== id ||
            swipeGesture.pointerId !== event.pointerId
        )
            return;
        const dx = event.clientX - swipeGesture.x;
        const dy = event.clientY - swipeGesture.y;
        swipeGesture = null;
        if (Math.abs(dx) < 45 || Math.abs(dx) <= Math.abs(dy) * 1.25) return;
        activeSwipeId = dx < 0 ? id : null;
        suppressedArticleClick = id;
        window.setTimeout(() => {
            if (suppressedArticleClick === id) suppressedArticleClick = null;
        }, 350);
    }

    function handleArticleClick(event: MouseEvent, id: number) {
        if (suppressedArticleClick === id) {
            event.preventDefault();
            suppressedArticleClick = null;
            return;
        }
        openArticle(id);
    }

    function handleArticleKeydown(event: KeyboardEvent, id: number) {
        const article = articles.find((item) => item.id === id);
        if (
            event.key === "ArrowLeft" &&
            article &&
            hasArticleActions(article)
        ) {
            event.preventDefault();
            activeSwipeId = id;
        } else if (event.key === "ArrowRight" || event.key === "Escape") {
            activeSwipeId = null;
        }
    }

    function focusDialog(node: HTMLElement, close: () => void) {
        const previousFocus = document.activeElement as HTMLElement | null;
        const fallbackFocus = previousFocus
            ?.closest(".capsule-wrap")
            ?.querySelector<HTMLButtonElement>(".article-open-button");
        const previousOverflow = document.body.style.overflow;
        document.body.style.overflow = "hidden";
        queueMicrotask(() => {
            if (node.isConnected) node.focus();
        });
        const handleKeydown = (event: KeyboardEvent) => {
            if (event.key === "Escape") {
                event.preventDefault();
                event.stopPropagation();
                close();
            } else if (event.key === "Tab") {
                const elements = Array.from(
                    node.querySelectorAll<HTMLElement>(
                        'button:not(:disabled), a[href], input:not(:disabled), textarea:not(:disabled), select:not(:disabled), [tabindex="0"]',
                    ),
                ).filter((element) => element.getClientRects().length > 0);
                const first = elements[0];
                const last = elements[elements.length - 1];
                if (!first) {
                    event.preventDefault();
                    node.focus();
                } else if (
                    event.shiftKey &&
                    (document.activeElement === first ||
                        document.activeElement === node)
                ) {
                    event.preventDefault();
                    last?.focus();
                } else if (
                    !event.shiftKey &&
                    (document.activeElement === last ||
                        document.activeElement === node)
                ) {
                    event.preventDefault();
                    first.focus();
                }
            }
        };
        node.addEventListener("keydown", handleKeydown);
        return {
            destroy() {
                node.removeEventListener("keydown", handleKeydown);
                document.body.style.overflow = previousOverflow;
                if (previousFocus?.isConnected) previousFocus.focus();
                else if (fallbackFocus?.isConnected) fallbackFocus.focus();
            },
        };
    }

    function focusAcknowledgment(node: HTMLElement) {
        queueMicrotask(() => {
            if (node.isConnected)
                node.querySelector<HTMLButtonElement>("button")?.focus();
        });
    }
    function deleteArticle(id: number) {
        const article = articles.find((item) => item.id === id);
        if (!article || !canDelete(article)) return;
        articles = articles.map((item) =>
            item.id === id ? { ...item, deleted: true } : item,
        );
        if (openArticleId === id) openArticleId = null;
        activeSwipeId = null;
        toastMessage = "Post moved to Deleted posts in your profile.";
        window.setTimeout(() => (toastMessage = ""), 4500);
    }
    function restoreArticle(id: number) {
        const article = articles.find((item) => item.id === id);
        if (
            isBlacklisted ||
            !article ||
            (!isAdmin && article.author !== accountName)
        )
            return;
        articles = articles.map((item) =>
            item.id === id ? { ...item, deleted: false } : item,
        );
    }
    function reportOpen(id: number) {
        const article = articles.find((item) => item.id === id);
        if (!article || !canReport(article)) return;
        reportArticle = id;
        reportText = "";
        reportMessage = "";
        reportSubmitted = false;
    }

    function submitReport(event: SubmitEvent) {
        event.preventDefault();
        const article = articles.find((item) => item.id === reportArticle);
        if (!article || !canReport(article)) return;
        if (!reportText.trim()) {
            reportMessage =
                "Please describe the concern before sending your report.";
            return;
        }
        reportSubmitted = true;
        reportMessage = "";
        reportText = "";
    }

    function factCheckOpen(id: number) {
        const article = articles.find((item) => item.id === id);
        if (!article || !canFactCheck(article)) return;
        factCheckArticle = id;
        factCheckSubmitted = false;
        factCheckedTitle = article.title;
        factCheckSources = ["", "", ""];
        factCheckFiles = [];
        factCheckMessage = "";
        factCheckAttempted = false;
        factFileError = "";
    }

    function closeFactCheck() {
        factCheckArticle = null;
        factCheckSubmitted = false;
        factCheckedTitle = "";
        factCheckSources = ["", "", ""];
        factCheckFiles = [];
        factCheckAttempted = false;
        factCheckMessage = "";
        factFileError = "";
    }

    function normalizeLink(value: string): string {
        try {
            const url = new URL(value.trim());
            url.hash = "";
            for (const key of [...url.searchParams.keys()]) {
                if (/^utm_/i.test(key) || key === "fbclid" || key === "gclid")
                    url.searchParams.delete(key);
            }
            url.searchParams.sort();
            const host = url.host.replace(/^www\./i, "");
            return `${host}${url.pathname.replace(/\/$/, "")}${url.search}`;
        } catch {
            return value.trim().toLowerCase();
        }
    }

    function isWebLink(value: string): boolean {
        try {
            const url = new URL(value.trim());
            return (
                (url.protocol === "http:" || url.protocol === "https:") &&
                url.hostname !== "" &&
                !url.username &&
                !url.password
            );
        } catch {
            return false;
        }
    }

    function factCheckLinkInvalid(index: number): boolean {
        const value = factCheckSources[index]?.trim() ?? "";
        return (
            (value !== "" &&
                duplicateFactLinks.includes(normalizeLink(value))) ||
            (factCheckAttempted && !isWebLink(value))
        );
    }

    function originalSourceInvalid(index: number): boolean {
        const value = createOriginalSources[index]?.trim() ?? "";
        return (
            (value !== "" &&
                duplicateOriginalSources.includes(normalizeLink(value))) ||
            (createOriginalSourcesAttempted && !isWebLink(value))
        );
    }

    function articleLinkInvalid(index: number): boolean {
        const value = createLinks[index]?.trim() ?? "";
        return (
            value !== "" &&
            (duplicateCreateLinks.includes(normalizeLink(value)) ||
                (createLinksAttempted && !isWebLink(value)))
        );
    }

    function setPostLink(index: number, event: Event) {
        createLinks[index] = (event.currentTarget as HTMLInputElement).value;
        createError = "";
    }

    function articleLinkError(index: number): string {
        if (duplicateCreateLinks.includes(normalizeLink(createLinks[index])))
            return "Use a different link. This source is already listed.";
        return "Enter a complete link beginning with http:// or https://.";
    }

    function addFileRecords(files: File[]): ArticleAttachment[] {
        return files.map((file) => ({
            name: file.name,
            type: file.type || "application/octet-stream",
            size: file.size,
            file,
        }));
    }

    function existingArticleStorageSize(articleId: number | null): number {
        const article = articles.find((item) => item.id === articleId);
        return [
            ...(article?.attachments ?? []),
            ...(article?.factCheckFiles ?? []),
        ].reduce((total, file) => total + file.size, 0);
    }

    function addFactFiles(event: Event) {
        if (isBlacklisted) return;
        const input = event.currentTarget as HTMLInputElement;
        const selected = Array.from(input.files ?? []);
        const currentFiles = factCheckFiles;
        const knownFiles = new Set(
            currentFiles.map(
                (file) => `${file.name.toLowerCase()}|${file.size}`,
            ),
        );
        const uniqueFiles = selected.filter((file) => {
            const key = `${file.name.toLowerCase()}|${file.size}`;
            if (knownFiles.has(key)) return false;
            knownFiles.add(key);
            return true;
        });
        const totalBytes =
            existingArticleStorageSize(factCheckArticle) +
            currentFiles.reduce((sum, file) => sum + file.size, 0) +
            uniqueFiles.reduce((sum, file) => sum + file.size, 0);
        if (totalBytes > MAX_POST_STORAGE_BYTES) {
            factFileError =
                "Attachments for a post may not exceed 10 MB in total.";
            input.value = "";
            return;
        }
        factFileError = "";
        factCheckFiles = [...factCheckFiles, ...addFileRecords(uniqueFiles)];
        input.value = "";
    }

    function removeFactFile(index: number) {
        factCheckFiles = factCheckFiles.filter(
            (_, fileIndex) => fileIndex !== index,
        );
        factFileError = "";
    }

    function submitFactCheck(event: SubmitEvent) {
        event.preventDefault();
        factCheckAttempted = true;
        factCheckMessage = "";
        const target = articles.find(
            (article) => article.id === factCheckArticle,
        );
        if (!target || !canFactCheck(target) || factCheckSubmitted) return;
        const sources = factCheckSources.map((source) => source.trim());
        if (
            sources.length !== 3 ||
            sources.some((source) => !isWebLink(source)) ||
            new Set(sources.map(normalizeLink)).size !== 3
        ) {
            factCheckMessage =
                "Each of the three source links must be valid and different.";
            return;
        }
        articles = articles.map((article) =>
            article.id === factCheckArticle
                ? {
                      ...article,
                      verified: true,
                      verifier: accountName,
                      verifiedAt:
                          new Date().toLocaleString("en-GB", {
                              day: "2-digit",
                              month: "short",
                              year: "numeric",
                              hour: "2-digit",
                              minute: "2-digit",
                              timeZone: "UTC",
                          }) + " UTC",
                      sources,
                      factCheckFiles: [
                          ...(article.factCheckFiles ?? []),
                          ...factCheckFiles,
                      ],
                  }
                : article,
        );
        factCheckSubmitted = true;
        activeSwipeId = null;
        factCheckSources = ["", "", ""];
        factCheckFiles = [];
        factCheckAttempted = false;
    }

    function addPostFiles(event: Event) {
        if (isBlacklisted) return;
        const input = event.currentTarget as HTMLInputElement;
        const selected = Array.from(input.files ?? []);
        const knownFiles = new Set(
            createFiles.map(
                (file) => `${file.name.toLowerCase()}|${file.size}`,
            ),
        );
        const uniqueFiles = selected.filter((file) => {
            const key = `${file.name.toLowerCase()}|${file.size}`;
            if (knownFiles.has(key)) return false;
            knownFiles.add(key);
            return true;
        });
        postFileWarning =
            uniqueFiles.length !== selected.length
                ? "A file with the same name and size is already attached."
                : "";
        const totalBytes =
            createFiles.reduce((sum, file) => sum + file.size, 0) +
            uniqueFiles.reduce((sum, file) => sum + file.size, 0);
        if (totalBytes > MAX_POST_STORAGE_BYTES) {
            postFileError =
                "Attachments for a post may not exceed 10 MB in total.";
            input.value = "";
            return;
        }
        postFileError = "";
        createFiles = [...createFiles, ...addFileRecords(uniqueFiles)];
        input.value = "";
    }

    function removePostFile(index: number) {
        createFiles = createFiles.filter((_, fileIndex) => fileIndex !== index);
        postFileError = "";
        postFileWarning = "";
    }

    function downloadAttachment(attachment: ArticleAttachment) {
        try {
            const url = URL.createObjectURL(attachment.file);
            const anchor = document.createElement("a");
            anchor.href = url;
            anchor.download = attachment.name;
            document.body.append(anchor);
            anchor.click();
            anchor.remove();
            window.setTimeout(() => URL.revokeObjectURL(url), 1500);
            downloadError = "";
        } catch {
            downloadError = `Could not download ${attachment.name}. Please try again.`;
            window.setTimeout(() => (downloadError = ""), 5000);
        }
    }

    function publishPost(event: SubmitEvent) {
        event.preventDefault();
        createError = "";
        createLinksAttempted = true;
        createOriginalSourcesAttempted = createOriginal;
        if (isBlacklisted) {
            createError =
                "This account cannot publish posts. Contact contact@foews.com to appeal.";
            return;
        }
        if (
            !createTitle.trim() ||
            !createExcerpt.trim() ||
            !createBody.trim()
        ) {
            createError =
                "Add a title, excerpt, and article text before publishing.";
            return;
        }
        const nonemptyLinks = createLinks
            .map((link) => link.trim())
            .filter(Boolean);
        if (
            nonemptyLinks.length > 3 ||
            nonemptyLinks.some((link) => !isWebLink(link))
        ) {
            createError =
                "Correct the highlighted article links before publishing.";
            return;
        }
        if (duplicateCreateLinks.length) {
            createError = "Use different article links before publishing.";
            return;
        }
        if (createOriginal && !isAdmin) {
            createError = "Only administrators can publish Foews Originals.";
            return;
        }
        const topic = currentTopic;
        const id = Date.now();
        const original = createOriginal;
        const originalSources = createOriginalSources.map((source) =>
            source.trim(),
        );
        if (
            original &&
            (!createAuthor.trim() ||
                !createVerifier.trim() ||
                originalSources.length !== 3 ||
                originalSources.some((source) => !isWebLink(source)) ||
                new Set(originalSources.map(normalizeLink)).size !== 3)
        ) {
            createError =
                "Add an author, an approved fact checker, and three different valid source links for this Original.";
            return;
        }
        articles = [
            {
                id,
                title: createTitle.trim(),
                excerpt: createExcerpt.trim(),
                body: createBody
                    .trim()
                    .split(/\n\s*\n/)
                    .filter(Boolean),
                region: original ? "originals" : createRegion,
                topic: original ? "originals" : topic.id,
                topicLabel: original ? "Foews Originals" : topic.name,
                author: original ? createAuthor.trim() : accountName,
                team: original ? "Foews Editorial" : "Community Post",
                published: "Just now",
                readTime: `${Math.max(1, Math.ceil(createBody.trim().split(/\s+/).length / 220))} min read`,
                likes: 0,
                liked: false,
                verified: original,
                verifier: original ? createVerifier : "",
                verifiedAt: original
                    ? new Date().toLocaleString("en-GB", {
                          day: "2-digit",
                          month: "short",
                          year: "numeric",
                          hour: "2-digit",
                          minute: "2-digit",
                          timeZone: "UTC",
                      }) + " UTC"
                    : "",
                sources: original ? originalSources : [],
                resources: nonemptyLinks,
                attachments: [...createFiles],
                factCheckFiles: [],
                gradient: original
                    ? "linear-gradient(125deg,#7209b7,#f72585)"
                    : topic.gradient,
            },
            ...articles,
        ];
        toastMessage = original ? "Original published." : "Post published.";
        window.setTimeout(() => (toastMessage = ""), 4500);
        createTitle = "";
        createExcerpt = "";
        createBody = "";
        createLinks = ["", "", ""];
        createFiles = [];
        createLinksAttempted = false;
        postFileError = "";
        postFileWarning = "";
        createOriginal = false;
        createAuthor = profileName;
        createVerifier = factCheckerRoster[0];
        createOriginalSources = ["", "", ""];
        createOriginalSourcesAttempted = false;
        createError = "";
        createRegion = "global";
        createTopic = "languages";
        regionMenuOpen = false;
        topicMenuOpen = false;
        verifierMenuOpen = false;
        selectedRegion = "create";
        scrollTop();
    }

    function changePassword(event: SubmitEvent) {
        event.preventDefault();
        if (isBlacklisted) return;
        passwordAttempted = true;
        passwordMessage = "";
        if (!currentPassword.trim()) {
            passwordMessage = "Enter your current password before updating it.";
            return;
        }
        if (!passwordValid) {
            passwordMessage =
                "Meet every password requirement before updating it.";
            return;
        }
        passwordEditing = false;
        currentPassword = "";
        newPassword = "";
        passwordMessage = "Password updated.";
        window.setTimeout(() => window.location.reload(), 500);
    }

    function saveProfile(event: SubmitEvent) {
        event.preventDefault();
        if (isBlacklisted) return;
        accountAttempted = true;
        accountSaved = false;
        accountMessage = "";
        if (!profileName.trim() || !namePattern.test(profileName.trim())) {
            accountMessage =
                "Use printable English ASCII characters in your display name.";
            return;
        }
        if (!emailPattern.test(profileEmail.trim())) {
            accountMessage = "Enter a valid email address.";
            return;
        }
        if (
            (phoneCountry.trim() || phoneNumber.trim()) &&
            (!countryValid || !phoneValid)
        ) {
            accountMessage =
                "Correct the phone country code and number before saving.";
            return;
        }
        try {
            localStorage.setItem(
                "foews-profile",
                JSON.stringify({
                    name: profileName.trim(),
                    email: profileEmail.trim(),
                    phoneCountry: phoneCountry.trim(),
                    phoneNumber: phoneNumber.trim(),
                }),
            );
        } catch {
            accountMessage =
                "Your browser could not save these profile changes.";
            return;
        }
        accountSaved = true;
        accountMessage = "Profile updated.";
        window.setTimeout(() => window.location.reload(), 500);
    }

    function formatExpiryInput(event: Event) {
        const input = event.currentTarget as HTMLInputElement;
        const digits = input.value.replace(/\D/g, "").slice(0, 4);
        cardExpiry =
            digits.length >= 2
                ? `${digits.slice(0, 2)}/${digits.slice(2)}`
                : digits;
        input.value = cardExpiry;
    }

    function showReportConfirmation() {
        reportArticle = null;
        reportSubmitted = false;
        reportMessage = "";
    }

    function startSubscription(event: SubmitEvent) {
        event.preventDefault();
        billingAttempted = true;
        if (isBlacklisted) {
            subscriptionMessage =
                "This restricted account cannot activate a membership. Contact contact@foews.com for an appeal.";
            subscriptionMessageError = true;
            return;
        }
        if (
            !billingName.trim() ||
            !namePattern.test(billingName.trim()) ||
            !emailPattern.test(billingEmail.trim()) ||
            !cardNumberValid() ||
            !expiryValid() ||
            !/^\d{3,4}$/.test(cardCvc.trim())
        ) {
            subscriptionMessage =
                "Correct the highlighted billing fields to continue.";
            subscriptionMessageError = true;
            return;
        }
        subscriptionActive = true;
        subscriptionMessage =
            "Your membership is active. Originals are now unlocked.";
        subscriptionMessageError = false;
        cardNumber = "";
        cardExpiry = "";
        cardCvc = "";
        billingAttempted = false;
        billingName = profileName;
        billingEmail = profileEmail;
        scrollTop();
    }
    function cancelSubscription() {
        if (isBlacklisted) return;
        subscriptionActive = false;
        subscriptionMessage =
            "Your membership has been cancelled. Originals are now locked.";
        subscriptionMessageError = false;
        if (selectedRegion === "originals") selectedRegion = "global";
        openArticleId = null;
        scrollTop();
    }
    function cardNumberValid() {
        const raw = cardNumber.trim();
        const digits = raw.replace(/\D/g, "");
        if (
            !/^[0-9\s-]+$/.test(raw) ||
            digits.length < 13 ||
            digits.length > 19
        )
            return false;
        let sum = 0;
        let doubleDigit = false;
        for (let index = digits.length - 1; index >= 0; index--) {
            let digit = Number(digits[index]);
            if (doubleDigit) {
                digit *= 2;
                if (digit > 9) digit -= 9;
            }
            sum += digit;
            doubleDigit = !doubleDigit;
        }
        return sum % 10 === 0;
    }
    function expiryValid() {
        const match = /^(0[1-9]|1[0-2])\/(\d{2})$/.exec(cardExpiry.trim());
        const now = new Date();
        const currentYear = Number(String(now.getUTCFullYear()).slice(-2));
        const currentMonth = now.getUTCMonth() + 1;
        return (
            !!match &&
            (Number(match[2]) > currentYear ||
                (Number(match[2]) === currentYear &&
                    Number(match[1]) >= currentMonth))
        );
    }
    function preventInvalidName(event: Event) {
        const input = event.currentTarget as HTMLInputElement;
        input.value = input.value.replace(/[^\x20-\x7E]/g, "");
    }
</script>

<svelte:head>
    <title>Foews | Technology without the noise</title>
    <meta
        name="description"
        content="Independent technology coverage across continents, engineering, and Foews Originals."
    />
    <meta name="theme-color" content="#faf9f6" />
</svelte:head>

{#snippet verificationDetails(article: Article)}
    <div class="verification-wrap">
        <button
            type="button"
            class="verification-icon"
            class:verified={article.verified}
            aria-label={article.verified
                ? `Fact-checked by ${article.verifier}. View sources.`
                : "Not yet fact-checked"}
            aria-haspopup="dialog"
            aria-controls={`verification-${article.id}`}
            onpointerenter={positionTooltip}
            onfocus={positionTooltip}
            onclick={positionTooltip}>{article.verified ? "✓" : "○"}</button
        >
        <div
            id={`verification-${article.id}`}
            class="verification-tooltip"
            role="dialog"
            aria-label="Editorial verification details"
        >
            <strong
                >{article.verified
                    ? "Fact-checked"
                    : "Not yet fact-checked"}</strong
            >
            {#if article.verified}
                <span>Verified by {article.verifier}</span>
                <span>{article.verifiedAt}</span>
                {#each article.sources as source (source)}
                    <a href={source} target="_blank" rel="noreferrer"
                        >{source}</a
                    >
                {/each}
                {#if article.factCheckFiles?.length}
                    <div class="tooltip-downloads">
                        {#each article.factCheckFiles as attachment, index (`${attachment.name}-${index}`)}
                            <button
                                type="button"
                                class="tooltip-download"
                                onclick={() => downloadAttachment(attachment)}
                                >↓ {attachment.name}</button
                            >
                        {/each}
                    </div>
                {/if}
            {:else}
                <span>This post has not completed editorial verification.</span>
            {/if}
        </div>
    </div>
{/snippet}

{#snippet articleCapsule(article: Article, personal: boolean)}
    <div
        class="capsule-wrap"
        class:actions-open={activeSwipeId === article.id &&
            hasArticleActions(article)}
    >
        <article
            class="article-capsule"
            class:swiped={activeSwipeId === article.id &&
                hasArticleActions(article)}
            style={`--article-gradient:${article.gradient}`}
        >
            <button
                type="button"
                class="article-open-button"
                aria-label={`Read ${article.title}`}
                onpointerdown={(event) => beginSwipe(event, article.id)}
                onpointerup={(event) => endSwipe(event, article.id)}
                onpointercancel={() => (swipeGesture = null)}
                onclick={(event) => handleArticleClick(event, article.id)}
                onkeydown={(event) => handleArticleKeydown(event, article.id)}
            ></button>
            <div class="article-visual">
                <span class="article-topic"
                    >{article.region === "originals"
                        ? "FOEWS ORIGINAL"
                        : article.topicLabel}</span
                >
                <span class="article-region"
                    >{personal
                        ? "YOUR POST"
                        : article.region === "originals"
                          ? "MEMBERS"
                          : (regionNames[article.region] ?? "World")}</span
                >
            </div>
            <div class="article-main">
                <div class="article-meta">
                    <span>{article.author}</span><span class="meta-dot">·</span
                    ><span>{article.published}</span><span class="meta-dot"
                        >·</span
                    ><span>{article.readTime}</span>
                </div>
                <h3>{article.title}</h3>
                {#if showExcerpts}<p
                        class="article-excerpt article-excerpt-collapsed"
                    >
                        {article.excerpt}
                    </p>{/if}
                <div class="article-footer">
                    <span class="article-team">{article.team}</span>
                    <div class="article-actions">
                        <button
                            type="button"
                            class="like-button"
                            class:liked={article.liked}
                            disabled={isBlacklisted}
                            aria-label={isBlacklisted
                                ? "Reactions are unavailable for this restricted account"
                                : "Toggle like"}
                            aria-pressed={article.liked}
                            onclick={() => toggleLike(article.id)}
                        >
                            <span class="heart-icon" aria-hidden="true"
                                >{article.liked ? "♥" : "♡"}</span
                            ><span class="like-count"
                                >{formatLikes(article.likes)}</span
                            >
                        </button>
                        {@render verificationDetails(article)}
                    </div>
                </div>
            </div>
            {#if hasArticleActions(article)}
                <button
                    type="button"
                    class="capsule-lip-trigger"
                    aria-label={activeSwipeId === article.id
                        ? "Close post actions"
                        : "Show post actions"}
                    aria-expanded={activeSwipeId === article.id}
                    aria-controls={`post-actions-${article.id}`}
                    onclick={() => toggleArticleActions(article.id)}
                >
                    <span aria-hidden="true"
                        >{activeSwipeId === article.id ? ">" : "<"}</span
                    >
                </button>
            {/if}
        </article>
        {#if hasArticleActions(article)}
            <div
                id={`post-actions-${article.id}`}
                class="capsule-actions"
                role="group"
                aria-label="Post actions"
                aria-hidden={activeSwipeId !== article.id}
                inert={activeSwipeId !== article.id}
            >
                {#if canReport(article)}
                    <button
                        type="button"
                        title="Report post"
                        aria-label="Report post"
                        onclick={() => reportOpen(article.id)}>🚩</button
                    >
                {/if}
                {#if canFactCheck(article)}
                    <button
                        type="button"
                        title="Fact-check post"
                        aria-label="Fact-check post"
                        onclick={() => factCheckOpen(article.id)}>🔎</button
                    >
                {/if}
                {#if canDelete(article)}
                    <button
                        type="button"
                        title="Delete post"
                        aria-label="Delete post"
                        onclick={() => deleteArticle(article.id)}>🗑️</button
                    >
                {/if}
            </div>
        {/if}
    </div>
{/snippet}

<div
    class="app-shell"
    class:compact={compactMode}
    class:excerpts-hidden={!showExcerpts}
>
    <aside class="sidebar">
        <div class="brand-wrap">
            <button
                type="button"
                class="brand"
                aria-label="Foews home"
                onclick={() => selectRegion("global")}
            >
                <span class="brand-mark">\f</span><span class="brand-equation"
                    >= <em>Foo + News</em></span
                >
            </button>
            <div
                class="brand-info"
                aria-label="Foews social profiles and contact"
            >
                <a
                    href="https://www.instagram.com/foewsnews"
                    target="_blank"
                    rel="noreferrer">Instagram <span>@foewsnews</span></a
                >
                <a
                    href="https://www.threads.com/@foewsnews"
                    target="_blank"
                    rel="noreferrer">Threads <span>@foewsnews</span></a
                >
                <a
                    href="https://x.com/foewsnews"
                    target="_blank"
                    rel="noreferrer">X <span>@foewsnews</span></a
                >
                <a
                    href="https://news.ycombinator.com/user?id=foewsnews"
                    target="_blank"
                    rel="noreferrer">Hacker News <span>foewsnews</span></a
                >
                <a
                    href="https://github.com/foewsnews"
                    target="_blank"
                    rel="noreferrer">GitHub <span>foewsnews</span></a
                >
                <a href="mailto:contact@foews.com"
                    >Email <span>contact@foews.com</span></a
                >
            </div>
        </div>
        <nav class="region-nav" aria-label="Navigation">
            {#each regions as region (region.id)}
                <button
                    type="button"
                    class="region-button"
                    disabled={isBlacklisted && region.id === "create"}
                    class:active={selectedRegion === region.id}
                    aria-label={region.label}
                    aria-current={selectedRegion === region.id
                        ? "page"
                        : undefined}
                    title={region.label}
                    onclick={() => selectRegion(region.id)}
                    ><span class="region-icon" aria-hidden="true"
                        >{region.icon}</span
                    ></button
                >
            {/each}
        </nav>
    </aside>

    <main class="workspace">
        <header class="topbar">
            <div class="topbar-left">
                {#if selectedArticle}<button
                        type="button"
                        class="back-link"
                        onclick={closeArticle}
                        aria-label="Back to articles">←</button
                    ><span class="region-name article-breadcrumb"
                        >{selectedArticle.title}</span
                    >
                {:else}{#if selectedRegion === "writers" && selectedWriter}<button
                            type="button"
                            class="back-link"
                            onclick={() => selectRegion("writers")}
                            aria-label="Back to all writers">←</button
                        >{/if}<span class="region-name"
                        >{selectedRegion === "writers" && selectedWriter
                            ? selectedWriter.name
                            : (regionNames[selectedRegion] ?? "World")}</span
                    >{#if selectedRegion !== "account" && selectedRegion !== "settings" && selectedRegion !== "originals" && selectedRegion !== "create" && activeTopic !== "world"}<span
                            class="context-separator">\</span
                        ><span class="context-topic"
                            >{currentTopics.find(
                                (topic) => topic.id === activeTopic,
                            )?.name}</span
                        >{/if}{/if}
            </div>
            <div class="topbar-right">
                <span class="wordmark-small">FOEWS</span>
            </div>
        </header>

        {#if selectedArticle}
            <article class="reader-page">
                <div class="reader-kicker">
                    <span>{selectedArticle.topicLabel}</span><span
                        class="meta-dot">·</span
                    ><span
                        >{regionNames[selectedArticle.region] ?? "Foews"}</span
                    >
                </div>
                <h1 class="reader-title">{selectedArticle.title}</h1>
                <p class="reader-deck">{selectedArticle.excerpt}</p>
                <div class="reader-byline">
                    <div class="reader-author">
                        <span class="author-avatar"
                            >{selectedArticle.author[0]?.toUpperCase() ??
                                "F"}</span
                        >
                        <div>
                            <strong>{selectedArticle.author}</strong><span
                                >{selectedArticle.team}</span
                            >
                        </div>
                    </div>
                    <div class="reader-publication">
                        <span>{selectedArticle.published}</span><span
                            >{selectedArticle.readTime}</span
                        >
                    </div>
                </div>
                <div class="reader-facts">
                    <div class="reader-fact">
                        <span class="fact-label">EDITORIAL STATUS</span><strong
                            class:verified-text={selectedArticle.verified}
                            class:pending-text={!selectedArticle.verified}
                            >{selectedArticle.verified
                                ? "✓ Fact-checked"
                                : "○ Not fact-checked"}</strong
                        >
                    </div>
                    <div class="reader-fact">
                        <span class="fact-label">VERIFICATION</span><strong
                            >{selectedArticle.verified
                                ? selectedArticle.verifier
                                : "Awaiting review"}</strong
                        >{#if selectedArticle.verified}<span
                                >{selectedArticle.verifiedAt}</span
                            >{/if}
                    </div>
                    <div class="reader-fact reader-reaction">
                        <span class="fact-label">READER REACTIONS</span><button
                            type="button"
                            class="like-button"
                            disabled={isBlacklisted}
                            aria-label="Like this article"
                            class:liked={selectedArticle.liked}
                            aria-pressed={selectedArticle.liked}
                            onclick={() => toggleLike(selectedArticle.id)}
                            ><span class="heart-icon"
                                >{selectedArticle.liked ? "♥" : "♡"}</span
                            ><span class="like-count visible"
                                >{formatLikes(selectedArticle.likes)}</span
                            ></button
                        >
                    </div>
                </div>
                <div class="reader-rule"></div>
                <div class="reader-content">
                    {#each selectedArticle.body as paragraph, index (index)}<p>
                            {paragraph}
                        </p>{/each}
                </div>
                {#if selectedArticle.resources?.length || selectedArticle.attachments?.length}<section
                        class="reader-resources"
                    >
                        {#if selectedArticle.resources?.length}<div>
                                <h2>Article links</h2>
                                <ul>
                                    {#each selectedArticle.resources as link (link)}<li
                                        >
                                            <a
                                                href={link}
                                                target="_blank"
                                                rel="noreferrer">{link}</a
                                            >
                                        </li>{/each}
                                </ul>
                            </div>{/if}{#if selectedArticle.attachments?.length}<div
                            >
                                <h2>Attached files</h2>
                                <div class="reader-attachments">
                                    {#each selectedArticle.attachments as attachment, index (`${attachment.name}-${index}`)}<button
                                            type="button"
                                            class="download-button"
                                            onclick={() =>
                                                downloadAttachment(attachment)}
                                            >↓ {attachment.name}</button
                                        >{/each}
                                </div>
                            </div>{/if}
                    </section>{/if}
                <div class="reader-end">
                    <span class="reader-end-mark">\f</span><span
                        >End of article</span
                    >
                </div>
            </article>
        {:else if selectedRegion === "writers"}
            <section class="hero">
                <div class="hero-copy">
                    <h1>{selectedWriter?.name ?? "Writers"}</h1>
                    {#if selectedWriter}<p>
                            Technical reporting and engineering perspectives
                            from {selectedWriter.name}.
                        </p>{:else}<p>
                            Meet the people behind the reporting.
                        </p>{/if}
                </div>
                <div class="hero-code">
                    <div class="code-header">
                        <span class="code-dots"
                            ><span></span><span></span><span></span></span
                        ><span class="code-filename"
                            >{currentHello.filename}</span
                        ><span class="code-language"
                            >{currentHello.language}</span
                        >
                    </div>
                    <pre><code>{currentHello.code}</code></pre>
                    <div class="code-footer">
                        <span>OUTPUT</span><span>Hello, world!</span>
                    </div>
                </div>
            </section>
            {#if selectedWriter}
                {#if writerTopics.length}
                    <section class="topic-section">
                        <div class="section-heading">
                            <h2>Topics</h2>
                            <span class="section-count"
                                >{writerTopics.length
                                    .toString()
                                    .padStart(2, "0")} topics</span
                            >
                        </div>
                        <div class="topic-grid">
                            {#each writerTopics as topic (topic.id)}
                                <button
                                    type="button"
                                    class="topic-card"
                                    class:selected={activeTopic === topic.id}
                                    style={`--topic-gradient:${topic.gradient}`}
                                    aria-pressed={activeTopic === topic.id}
                                    onclick={() => selectTopic(topic.id)}
                                    ><span class="topic-name">{topic.name}</span
                                    ></button
                                >
                            {/each}
                        </div>
                    </section>
                {/if}
                <section class="coverage-section">
                    <div class="section-heading coverage-heading">
                        <div>
                            <h2>
                                {activeTopic === "world"
                                    ? "Latest posts"
                                    : (currentTopics.find(
                                          (topic) => topic.id === activeTopic,
                                      )?.name ?? "Latest posts")}
                            </h2>
                            <p class="section-description">
                                Published work by {selectedWriter.name}.
                            </p>
                        </div>
                        <span class="section-count"
                            >{visibleArticles.length
                                .toString()
                                .padStart(2, "0")} stories</span
                        >
                    </div>
                    {#if visibleArticles.length}
                        <div class="article-stack">
                            {#each visibleArticles as article (article.id)}
                                {@render articleCapsule(article, false)}
                            {/each}
                        </div>
                    {:else if !lockedWriterOriginals}
                        <div class="no-results">
                            <h3>No matching posts</h3>
                            <p>
                                There are no published posts to show in this
                                view.
                            </p>
                            {#if activeTopic !== "world"}<button
                                    type="button"
                                    class="primary-button"
                                    onclick={() => (activeTopic = "world")}
                                    >Show all posts</button
                                >{:else}<button
                                    type="button"
                                    class="primary-button"
                                    onclick={() => selectRegion("writers")}
                                    >All writers</button
                                >{/if}
                        </div>
                    {/if}
                    {#if lockedWriterOriginals}
                        <div class="writer-membership">
                            <div>
                                <strong>Foews Originals</strong>
                                <p>
                                    {lockedWriterOriginals}
                                    {lockedWriterOriginals === 1
                                        ? "Original by this writer is"
                                        : "Originals by this writer are"} available
                                    with an Originals membership.
                                </p>
                            </div>
                            <button
                                type="button"
                                class="secondary-button"
                                onclick={() => selectRegion("account")}
                                >View membership options</button
                            >
                        </div>
                    {/if}
                </section>
            {:else}
                <section class="coverage-section">
                    <div class="section-heading">
                        <div>
                            <h2>All writers</h2>
                            <p class="section-description">
                                Browse by first name or find someone below.
                            </p>
                        </div>
                        <span
                            class="section-count"
                            role="status"
                            aria-live="polite"
                            >{filteredWriters.length
                                .toString()
                                .padStart(2, "0")}
                            {filteredWriters.length === 1
                                ? "writer"
                                : "writers"}</span
                        >
                    </div>
                    <div
                        class="writer-search-capsule"
                        role="search"
                        aria-label="Find writers"
                    >
                        <div class="section-heading">
                            <div>
                                <h2>Find a writer</h2>
                                <p class="section-description">
                                    Filter by first and last name as you type.
                                </p>
                            </div>
                        </div>
                        <div class="writer-search-fields">
                            <label class="profile-field"
                                ><span class="field-label">First name</span
                                ><input
                                    type="text"
                                    bind:value={writerFirstName}
                                    placeholder="First name"
                                    autocomplete="off"
                                    aria-controls="writer-results"
                                /></label
                            >
                            <label class="profile-field"
                                ><span class="field-label">Last name</span
                                ><input
                                    type="text"
                                    bind:value={writerLastName}
                                    placeholder="Last name"
                                    autocomplete="off"
                                    aria-controls="writer-results"
                                /></label
                            >
                        </div>
                    </div>
                    <div id="writer-results" class="writer-groups">
                        {#if writerGroups.length}
                            {#each writerGroups as group (group.letter)}
                                <section
                                    class="writer-group"
                                    aria-labelledby={`writer-letter-${group.letter.codePointAt(0)}`}
                                >
                                    <div class="writer-letter-heading">
                                        <h3
                                            id={`writer-letter-${group.letter.codePointAt(0)}`}
                                        >
                                            {group.letter}
                                        </h3>
                                    </div>
                                    <div class="writer-grid">
                                        {#each group.members as writer (writer.id)}
                                            <button
                                                type="button"
                                                class="topic-card writer-card"
                                                style={`--topic-gradient:${writer.gradient}`}
                                                aria-label={`View posts by ${writer.name}`}
                                                onclick={() =>
                                                    selectWriter(writer.id)}
                                                ><span class="topic-name"
                                                    >{writer.name}</span
                                                ></button
                                            >
                                        {/each}
                                    </div>
                                </section>
                            {/each}
                        {:else}
                            <div class="no-results">
                                <h3>Sorry, we couldn’t find that writer.</h3>
                                <p>Try another first or last name.</p>
                                <button
                                    type="button"
                                    class="primary-button"
                                    onclick={() => {
                                        writerFirstName = "";
                                        writerLastName = "";
                                    }}>Clear name filters</button
                                >
                            </div>
                        {/if}
                    </div>
                </section>
            {/if}
        {:else if selectedRegion === "account"}
            <section class="hero account-hero">
                <div class="hero-copy">
                    <span class="eyebrow">ACCOUNT</span>
                    <h1>{profileName || "Your profile"}</h1>
                    <p>
                        Your account, editorial preferences, subscription, and
                        published work.
                    </p>
                </div>
                <div class="profile-summary">
                    <span class="profile-initial"
                        >{profileName.trim()[0]?.toUpperCase() || "U"}</span
                    >
                    <span class="profile-summary-label">MEMBER PROFILE</span>
                    <span class="profile-summary-name"
                        >{profileName || "Unnamed User"}</span
                    >
                    {#if isBlacklisted}
                        <button
                            type="button"
                            class="subscription-badge banned-badge"
                            aria-expanded={showBlacklistMessage}
                            onclick={() =>
                                (showBlacklistMessage = !showBlacklistMessage)}
                            >BLACKLISTED ACCOUNT</button
                        >
                        {#if showBlacklistMessage}<div
                                class="blacklist-message"
                            >
                                <strong
                                    >Account restricted by the Foews team</strong
                                ><span
                                    >This account was found not to follow the
                                    community guidelines, including respectful
                                    language and the site's technology focus.
                                    For an appeal, contact <a
                                        href="mailto:contact@foews.com"
                                        >contact@foews.com</a
                                    >.</span
                                >
                            </div>{/if}
                    {:else}
                        <span
                            class="subscription-badge"
                            class:subscription-active={subscriptionActive ||
                                isAdmin}
                            >{isAdmin
                                ? "FOEWS ADMIN"
                                : isFactChecker
                                  ? "FACT CHECKER"
                                  : subscriptionActive
                                    ? "ORIGINALS ACTIVE"
                                    : "FREE ACCOUNT"}</span
                        >
                        {#if isFactChecker}<span class="role-note"
                                >Fact-checking access enabled</span
                            >{/if}
                    {/if}
                </div>
            </section>
            <section class="profile-section">
                <div class="section-heading">
                    <div>
                        <h2>Personal details</h2>
                        <p class="section-description">
                            Manage your public identity and account information.
                        </p>
                    </div>
                </div>
                <form class="profile-form" novalidate onsubmit={saveProfile}>
                    <label class="profile-field"
                        ><span class="field-label">Display name</span><input
                            disabled={isBlacklisted}
                            class:invalid={accountAttempted &&
                                (!profileName.trim() ||
                                    !namePattern.test(profileName.trim()))}
                            bind:value={profileName}
                            oninput={(event) => {
                                preventInvalidName(event);
                                accountSaved = false;
                                accountMessage = "";
                            }}
                            placeholder="Your name"
                            autocomplete="name"
                            required
                        />{#if accountAttempted && (!profileName.trim() || !namePattern.test(profileName.trim()))}<span
                                class="field-help error-text"
                                >Enter a name using printable English ASCII
                                characters.</span
                            >{:else}<span class="field-help"
                                >Printable English ASCII characters only.</span
                            >{/if}</label
                    >
                    <label class="profile-field"
                        ><span class="field-label">Email address</span><input
                            disabled={isBlacklisted}
                            type="email"
                            class:invalid={accountAttempted &&
                                !emailPattern.test(profileEmail.trim())}
                            bind:value={profileEmail}
                            oninput={() => {
                                accountSaved = false;
                                accountMessage = "";
                            }}
                            placeholder="you@example.com"
                            autocomplete="email"
                            required
                        />{#if accountAttempted && !emailPattern.test(profileEmail.trim())}<span
                                class="field-help error-text"
                                >Enter a valid email address.</span
                            >{:else}<span class="field-help"
                                >Account email and role access.</span
                            >{/if}</label
                    >
                    <label class="profile-field phone-field"
                        ><span class="field-label"
                            >Phone number <span class="field-inline-note"
                                >Two-factor authentication</span
                            ></span
                        ><span class="phone-inputs"
                            ><input
                                disabled={isBlacklisted}
                                class:invalid={accountAttempted &&
                                    (phoneCountry.trim() !== "" ||
                                        phoneNumber.trim() !== "") &&
                                    !countryValid}
                                class:warning={phoneWarning}
                                bind:value={phoneCountry}
                                oninput={() => {
                                    accountMessage = "";
                                    accountSaved = false;
                                }}
                                inputmode="tel"
                                placeholder="+1"
                                aria-label="Country calling code"
                                autocomplete="tel-country-code"
                            /><input
                                disabled={isBlacklisted}
                                class:invalid={accountAttempted &&
                                    (phoneCountry.trim() !== "" ||
                                        phoneNumber.trim() !== "") &&
                                    !phoneValid}
                                class:warning={phoneWarning}
                                bind:value={phoneNumber}
                                oninput={() => {
                                    accountMessage = "";
                                    accountSaved = false;
                                }}
                                inputmode="tel"
                                placeholder="555 010 2040"
                                aria-label="Phone number"
                                autocomplete="tel-national"
                            /></span
                        >{#if phoneWarning}<span class="field-help warning-text"
                                >⚠ Two-factor authentication is not turned on.
                                Add a phone number to enable it.</span
                            >{:else if accountAttempted && (phoneCountry.trim() || phoneNumber.trim()) && (!countryValid || !phoneValid)}<span
                                class="field-help error-text"
                                >Enter a valid country calling code and a 6 to
                                15 digit phone number.</span
                            >{:else if phoneSet}<span
                                class="field-help success-text"
                                >✓ Phone number set. Two-factor authentication
                                is on.</span
                            >{:else}<span class="field-help"
                                >Use a country calling code such as +1, +44, or
                                +64.</span
                            >{/if}</label
                    >
                    <div class="profile-form-footer">
                        <button
                            class="primary-button"
                            type="submit"
                            disabled={isBlacklisted}>Save changes</button
                        >{#if accountSaved}<span
                                class="save-message"
                                role="status">Profile updated.</span
                            >{/if}
                    </div>
                </form>
                {#if accountMessage}<p
                        class="inline-message"
                        class:error-text={!accountMessage.endsWith("updated.")}
                        role="status"
                    >
                        {accountMessage}
                    </p>{/if}
            </section>
            <section class="profile-section">
                <div class="section-heading">
                    <div>
                        <h2>Password</h2>
                        <p class="section-description">
                            Confirm your current password before choosing a new
                            one.
                        </p>
                    </div>
                </div>
                {#if !passwordEditing}<div class="password-summary">
                        <span>Password is hidden for your security.</span
                        ><button
                            type="button"
                            class="secondary-button"
                            disabled={isBlacklisted}
                            onclick={() => {
                                passwordEditing = true;
                                passwordAttempted = false;
                                passwordMessage = "";
                            }}>Change password</button
                        >
                    </div>
                {:else}<form
                        class="profile-form password-form"
                        novalidate
                        onsubmit={changePassword}
                    >
                        <label class="profile-field"
                            ><span class="field-label">Current password</span
                            ><span class="password-input"
                                ><input
                                    type={showCurrentPassword
                                        ? "text"
                                        : "password"}
                                    class:invalid={passwordAttempted &&
                                        !currentPassword.trim()}
                                    bind:value={currentPassword}
                                    autocomplete="current-password"
                                    required
                                /><button
                                    type="button"
                                    class="reveal-button"
                                    aria-label={showCurrentPassword
                                        ? "Hide current password"
                                        : "Show current password"}
                                    onclick={() =>
                                        (showCurrentPassword =
                                            !showCurrentPassword)}
                                    >{showCurrentPassword ? "🙈" : "👁️"}</button
                                ></span
                            >{#if passwordAttempted && !currentPassword.trim()}<span
                                    class="field-help error-text"
                                    >Enter your current password.</span
                                >{/if}</label
                        ><label class="profile-field"
                            ><span class="field-label">New password</span><span
                                class="password-input"
                                ><input
                                    type={showNewPassword ? "text" : "password"}
                                    class:invalid={newPassword.length > 0 &&
                                        !passwordValid}
                                    bind:value={newPassword}
                                    autocomplete="new-password"
                                    required
                                /><button
                                    type="button"
                                    class="reveal-button"
                                    aria-label={showNewPassword
                                        ? "Hide new password"
                                        : "Show new password"}
                                    onclick={() =>
                                        (showNewPassword = !showNewPassword)}
                                    >{showNewPassword ? "🙈" : "👁️"}</button
                                ></span
                            ><span class="password-rules"
                                >{#each passwordRules as rule (rule.label)}<span
                                        class:rule-met={rule.test(newPassword)}
                                        class:rule-failed={newPassword.length >
                                            0 && !rule.test(newPassword)}
                                        >{rule.test(newPassword) ? "✓" : "○"}
                                        {rule.label}</span
                                    >{/each}</span
                            ></label
                        >
                        <div class="profile-form-footer">
                            <button
                                class="primary-button"
                                type="submit"
                                disabled={!passwordValid}
                                >Update password</button
                            ><button
                                class="secondary-button"
                                type="button"
                                onclick={() => {
                                    passwordEditing = false;
                                    passwordAttempted = false;
                                    currentPassword = "";
                                    newPassword = "";
                                }}>Cancel</button
                            >
                        </div>
                    </form>{/if}
                {#if passwordMessage}<p
                        class="inline-message"
                        class:error-text={!passwordMessage.endsWith("updated.")}
                        role="status"
                    >
                        {passwordMessage}
                    </p>{/if}
            </section>
            <section class="profile-section subscription-section">
                <div class="section-heading">
                    <div>
                        <h2>Foews Originals subscription</h2>
                        <p class="section-description">
                            Investigations, original interviews, and editorial
                            deep dives produced for Foews readers.
                        </p>
                    </div>
                </div>
                {#if subscriptionActive || isAdmin}<div
                        class="subscription-active-card"
                    >
                        <div class="subscription-card-main">
                            <span class="subscription-symbol">✍️</span>
                            <div>
                                <span class="subscription-status-label"
                                    >{isAdmin
                                        ? "ADMIN ACCESS"
                                        : "SUBSCRIPTION STATUS"}</span
                                >
                                <h3>Foews Originals</h3>
                                <p>
                                    {isAdmin
                                        ? "Included with your administrator role."
                                        : "Your membership provides access to the Originals collection."}
                                </p>
                            </div>
                        </div>
                        <div class="subscription-details">
                            <div>
                                <span>PLAN</span><strong
                                    >{isAdmin
                                        ? "Team member access"
                                        : "Originals Membership"}</strong
                                >
                            </div>
                            <div>
                                <span>ACCESS</span><strong
                                    >{isAdmin
                                        ? "Administrator"
                                        : billingEmail || profileEmail}</strong
                                >
                            </div>
                            <div>
                                <span>PAYMENT STATUS</span><strong
                                    >{isAdmin
                                        ? "Not required"
                                        : "Checkout is not connected"}</strong
                                >
                            </div>
                        </div>
                        <div class="subscription-actions">
                            <button
                                type="button"
                                class="primary-button"
                                onclick={() => selectRegion("originals")}
                                >Read Originals</button
                            >{#if subscriptionActive && !isAdmin}<button
                                    type="button"
                                    class="secondary-button"
                                    disabled={isBlacklisted}
                                    onclick={cancelSubscription}
                                    >Cancel membership</button
                                >{/if}
                        </div>
                    </div>
                {:else}<div class="subscription-offer">
                        <div class="offer-topline">
                            <span class="offer-kicker"
                                >THE FOEWS ORIGINALS COLLECTION</span
                            ><span class="offer-label">MEMBERSHIP</span>
                        </div>
                        <h3>Reporting that goes deeper.</h3>
                        <p>
                            Access original investigations, long-form technical
                            analysis, and interviews commissioned by the Foews
                            editorial team. Every published Original carries
                            editorial verification metadata.
                        </p>
                        <div class="offer-benefits">
                            <div>
                                <span>✓</span> Original investigations and interviews
                            </div>
                            <div>
                                <span>✓</span> Long-form technical reporting
                            </div>
                            <div>
                                <span>✓</span> Editorial fact-check information
                            </div>
                            <div>
                                <span>✓</span> Full access to the Originals archive
                            </div>
                        </div>
                        <div class="offer-price">
                            <strong>€4.99</strong><span>/ month</span>
                        </div>
                        <form
                            class="billing-form"
                            novalidate
                            onsubmit={startSubscription}
                        >
                            <div class="billing-heading">
                                <h4>Membership details</h4>
                                <span>MEMBERSHIP CHECKOUT</span>
                            </div>
                            <label class="profile-field"
                                ><span class="field-label">Billing name</span
                                ><input
                                    disabled={isBlacklisted}
                                    bind:value={billingName}
                                    oninput={(event) => {
                                        preventInvalidName(event);
                                        clearBillingMessage();
                                    }}
                                    class:invalid={billingAttempted &&
                                        (!billingName.trim() ||
                                            !namePattern.test(
                                                billingName.trim(),
                                            ))}
                                    autocomplete="cc-name"
                                />{#if billingAttempted && (!billingName.trim() || !namePattern.test(billingName.trim()))}<span
                                        class="field-help error-text"
                                        >Enter a name using printable English
                                        ASCII characters.</span
                                    >{/if}</label
                            ><label class="profile-field"
                                ><span class="field-label">Billing email</span
                                ><input
                                    disabled={isBlacklisted}
                                    type="email"
                                    bind:value={billingEmail}
                                    oninput={clearBillingMessage}
                                    class:invalid={billingAttempted &&
                                        !emailPattern.test(billingEmail.trim())}
                                    autocomplete="email"
                                />{#if billingAttempted && !emailPattern.test(billingEmail.trim())}<span
                                        class="field-help error-text"
                                        >Enter a valid email address.</span
                                    >{/if}</label
                            ><label class="profile-field card-number-field"
                                ><span class="field-label">Card number</span
                                ><input
                                    disabled={isBlacklisted}
                                    bind:value={cardNumber}
                                    oninput={clearBillingMessage}
                                    inputmode="numeric"
                                    autocomplete="off"
                                    placeholder="4242 4242 4242 4242"
                                    class:invalid={billingAttempted &&
                                        !cardNumberValid()}
                                /><span class="field-help"
                                    >Enter a valid card number.</span
                                >{#if billingAttempted && !cardNumberValid()}<span
                                        class="field-help error-text"
                                        >Enter a valid 13 to 19 digit card
                                        number.</span
                                    >{/if}</label
                            >
                            <div class="billing-row">
                                <label class="profile-field"
                                    ><span class="field-label">Expiry date</span
                                    ><input
                                        disabled={isBlacklisted}
                                        bind:value={cardExpiry}
                                        inputmode="numeric"
                                        placeholder="MM/YY"
                                        class:invalid={billingAttempted &&
                                            !expiryValid()}
                                        oninput={(event) => {
                                            formatExpiryInput(event);
                                            clearBillingMessage();
                                        }}
                                    />{#if billingAttempted && !expiryValid()}<span
                                            class="field-help error-text"
                                            >Enter a valid future date as MM/YY.</span
                                        >{/if}</label
                                ><label class="profile-field"
                                    ><span class="field-label">CVC</span><input
                                        disabled={isBlacklisted}
                                        bind:value={cardCvc}
                                        oninput={clearBillingMessage}
                                        inputmode="numeric"
                                        placeholder="123"
                                        class:invalid={billingAttempted &&
                                            !/^\d{3,4}$/.test(cardCvc.trim())}
                                    />{#if billingAttempted && !/^\d{3,4}$/.test(cardCvc.trim())}<span
                                            class="field-help error-text"
                                            >Enter the 3 or 4 digit security
                                            code.</span
                                        >{/if}</label
                                >
                            </div>
                            <button
                                class="primary-button subscribe-button"
                                disabled={isBlacklisted}
                                type="submit"
                                >Activate membership <span aria-hidden="true"
                                    >→</span
                                ></button
                            >
                            <p class="billing-disclaimer">
                                Checkout is not connected to a payment provider.
                                Card details are not sent or stored, and no
                                payment can be taken.
                            </p>
                        </form>
                    </div>{/if}
                {#if subscriptionMessage}<p
                        class="subscription-message"
                        class:error-text={subscriptionMessageError}
                        role="status"
                    >
                        {subscriptionMessage}
                    </p>{/if}
            </section>
            <section class="profile-section">
                <div class="section-heading">
                    <div>
                        <h2>Your posts</h2>
                        <p class="section-description">
                            Your published engineering notes and articles.
                        </p>
                    </div>
                    <span class="section-count"
                        >{profilePosts.length.toString().padStart(2, "0")} posts</span
                    >
                </div>
                {#if profilePosts.length}
                    <div class="article-stack">
                        {#each profilePosts as article (article.id)}
                            {@render articleCapsule(article, true)}
                        {/each}
                    </div>
                {:else}
                    <div class="no-results">
                        <h3>No posts yet</h3>
                        <p>Your published articles will appear here.</p>
                        {#if !isBlacklisted}<button
                                type="button"
                                class="primary-button"
                                onclick={() => selectRegion("create")}
                                >Create a post</button
                            >{/if}
                    </div>
                {/if}
            </section>
            <section class="profile-section deleted-posts-section">
                <div class="section-heading">
                    <div>
                        <h2>Deleted posts</h2>
                        <p class="section-description">
                            Restore posts removed from your profile or the Foews
                            feed.
                        </p>
                    </div>
                    <span class="section-count"
                        >{deletedArticles.length.toString().padStart(2, "0")} items</span
                    >
                </div>
                {#if deletedArticles.length}<div class="article-stack">
                        {#each deletedArticles as article (article.id)}<article
                                class="article-capsule deleted-capsule"
                                style={`--article-gradient:${article.gradient}`}
                            >
                                <div class="article-visual">
                                    <span class="article-topic"
                                        >{article.topicLabel}</span
                                    ><span class="article-region"
                                        >DELETED POST</span
                                    >
                                </div>
                                <div class="article-main">
                                    <div class="article-meta">
                                        <span>{article.author}</span><span
                                            class="meta-dot">·</span
                                        ><span>{article.published}</span><span
                                            class="meta-dot">·</span
                                        ><span>{article.readTime}</span>
                                    </div>
                                    <h3>{article.title}</h3>
                                    {#if showExcerpts}<p
                                            class="article-excerpt"
                                        >
                                            {article.excerpt}
                                        </p>{/if}
                                    <div class="article-footer">
                                        <span class="article-team"
                                            >{article.team}</span
                                        ><button
                                            class="restore-button"
                                            type="button"
                                            disabled={isBlacklisted}
                                            onclick={() =>
                                                restoreArticle(article.id)}
                                            >Restore</button
                                        >
                                    </div>
                                </div>
                            </article>{/each}
                    </div>{:else}<div class="empty-deleted">
                        <span>🗑️</span>
                        <p>No deleted posts.</p>
                    </div>{/if}
            </section>
        {:else if selectedRegion === "create" && isBlacklisted}
            <section class="no-results">
                <span class="no-results-symbol">\f</span>
                <h3>Reading access only</h3>
                <p>
                    This account cannot publish or interact with posts. Contact <a
                        href="mailto:contact@foews.com">contact@foews.com</a
                    > to appeal.
                </p>
            </section>
        {:else if selectedRegion === "create"}
            <section class="hero create-hero">
                <div class="hero-copy">
                    <span class="eyebrow">PUBLISH</span>
                    <h1>Create a post</h1>
                    <p>Share a technical story with Foews readers.</p>
                </div>
                <div class="settings-summary">
                    <span class="settings-summary-label">PUBLISHING AS</span
                    ><span class="settings-summary-value">{profileName}</span
                    ><span class="settings-summary-detail"
                        >{isAdmin
                            ? "Administrator access"
                            : isFactChecker
                              ? "Fact-checker access"
                              : "Community contributor"}</span
                    >
                </div>
            </section>
            <section class="profile-section">
                <div class="section-heading">
                    <div>
                        <h2>Post details</h2>
                        <p class="section-description">
                            Choose where it belongs, then write your article.
                        </p>
                    </div>
                </div>
                <form class="editor-form" novalidate onsubmit={publishPost}>
                    <label class="profile-field"
                        ><span class="field-label">Title</span><input
                            bind:value={createTitle}
                            maxlength="140"
                            class:invalid={createError !== "" &&
                                !createTitle.trim()}
                            placeholder="Give your post a clear title"
                        />{#if createError !== "" && !createTitle.trim()}<span
                                class="field-help error-text"
                                >Add a title before publishing.</span
                            >{:else}<span class="field-help"
                                >A concise headline for your story.</span
                            >{/if}</label
                    >
                    <div class="editor-select-grid">
                        <div class="profile-field">
                            <span class="field-label">Region</span>
                            <div class="custom-select">
                                <button
                                    type="button"
                                    class="select-trigger"
                                    aria-haspopup="listbox"
                                    aria-expanded={regionMenuOpen}
                                    onclick={() => {
                                        regionMenuOpen = !regionMenuOpen;
                                        topicMenuOpen = false;
                                        verifierMenuOpen = false;
                                    }}
                                    ><span
                                        >{regions.find(
                                            (region) =>
                                                region.id === createRegion,
                                        )?.icon}
                                        {regions.find(
                                            (region) =>
                                                region.id === createRegion,
                                        )?.label}</span
                                    ><span class="select-chevron">⌄</span
                                    ></button
                                >{#if regionMenuOpen}<div
                                        class="select-menu"
                                        role="listbox"
                                        aria-label="Choose a region"
                                    >
                                        {#each regions.filter((region) => !["originals", "account", "settings", "create", "writers"].includes(region.id)) as region (region.id)}<button
                                                type="button"
                                                role="option"
                                                aria-selected={createRegion ===
                                                    region.id}
                                                class="region-option"
                                                class:selected={createRegion ===
                                                    region.id}
                                                onclick={() => {
                                                    createRegion = region.id;
                                                    regionMenuOpen = false;
                                                }}
                                                ><span>{region.icon}</span><span
                                                    >{region.label}</span
                                                ></button
                                            >{/each}
                                    </div>{/if}
                            </div>
                        </div>
                        <div class="profile-field">
                            <span class="field-label">Topic</span>
                            <div class="custom-select">
                                <button
                                    type="button"
                                    class="select-trigger topic-select-trigger"
                                    style={`--topic-gradient:${currentTopic?.gradient ?? "linear-gradient(125deg,#3a0ca3,#4cc9f0)"}`}
                                    aria-haspopup="listbox"
                                    aria-expanded={topicMenuOpen}
                                    onclick={() => {
                                        topicMenuOpen = !topicMenuOpen;
                                        regionMenuOpen = false;
                                        verifierMenuOpen = false;
                                    }}
                                    ><span
                                        class="selected-topic-pill"
                                        style={`--topic-gradient:${currentTopic?.gradient ?? "linear-gradient(125deg,#3a0ca3,#4cc9f0)"}`}
                                        >{currentTopic?.name ??
                                            "Choose a topic"}</span
                                    ><span class="select-chevron">⌄</span
                                    ></button
                                >{#if topicMenuOpen}<div
                                        class="topic-select-menu"
                                        role="listbox"
                                        aria-label="Choose a topic"
                                    >
                                        {#each createTopics as topic (topic.id)}<button
                                                type="button"
                                                role="option"
                                                aria-selected={createTopic ===
                                                    topic.id}
                                                class="topic-option"
                                                class:selected={createTopic ===
                                                    topic.id}
                                                style={`--topic-gradient:${topic.gradient}`}
                                                onclick={() => {
                                                    createTopic = topic.id;
                                                    topicMenuOpen = false;
                                                }}
                                                ><span>{topic.name}</span
                                                ></button
                                            >{/each}
                                    </div>{/if}
                            </div>
                        </div>
                    </div>
                    <label class="profile-field"
                        ><span class="field-label">Excerpt</span><textarea
                            bind:value={createExcerpt}
                            rows="3"
                            maxlength="320"
                            class:invalid={createError !== "" &&
                                !createExcerpt.trim()}
                            placeholder="Summarize the main idea of your article in a few sentences."
                        ></textarea>{#if createError !== "" && !createExcerpt.trim()}<span
                                class="field-help error-text"
                                >Add a short summary before publishing.</span
                            >{:else}<span class="field-help"
                                >A short summary shown on the article capsule.</span
                            >{/if}</label
                    >
                    <label class="profile-field"
                        ><span class="field-label">Article text</span><textarea
                            bind:value={createBody}
                            rows="13"
                            class:invalid={createError !== "" &&
                                !createBody.trim()}
                            placeholder="Write your article here. Separate paragraphs with a blank line."
                        ></textarea>{#if createError !== "" && !createBody.trim()}<span
                                class="field-help error-text"
                                >Add your article text before publishing.</span
                            >{:else}<span class="field-help"
                                >Your post will show your name and the region
                                and topic you selected.</span
                            >{/if}</label
                    >
                    <div class="resource-editor">
                        <div class="resource-editor-heading">
                            <div>
                                <strong>Links at the end of your article</strong
                                ><span
                                    >Add up to three different sources or
                                    related links.</span
                                >
                            </div>
                            <span class="section-count">UP TO 3</span>
                        </div>
                        {#each createLinks as link, index (index)}
                            <label class="profile-field link-field">
                                <span class="field-label">Link {index + 1}</span
                                >
                                <input
                                    type="url"
                                    value={link}
                                    oninput={(event) =>
                                        setPostLink(index, event)}
                                    class:invalid={articleLinkInvalid(index)}
                                    aria-invalid={articleLinkInvalid(index)}
                                    placeholder="https://example.com/article"
                                />
                                {#if articleLinkInvalid(index)}<span
                                        class="field-help error-text"
                                        >{articleLinkError(index)}</span
                                    >{/if}
                            </label>
                        {/each}
                    </div>
                    <div class="resource-editor">
                        <div class="resource-editor-heading">
                            <div>
                                <strong>Files attached to your article</strong
                                ><span
                                    >Up to 10 MB total per post. You can attach
                                    multiple files.</span
                                >
                            </div>
                        </div>
                        <input
                            id="create-files"
                            class="file-input-hidden"
                            type="file"
                            multiple
                            accept="image/*,.pdf,.txt,.csv,.doc,.docx"
                            onchange={addPostFiles}
                        /><label
                            class="file-picker"
                            class:invalid={postFileError !== ""}
                            for="create-files"
                            ><span aria-hidden="true">📎</span> Add files</label
                        ><span class="field-help"
                            >{(createTotalFileBytes / (1024 * 1024)).toFixed(2)}
                            MB of 10 MB used.</span
                        >{#if postFileWarning}<p
                                class="inline-warning"
                                role="status"
                            >
                                ⚠ {postFileWarning}
                            </p>{/if}{#if createFiles.length}<div
                                class="attachment-list"
                            >
                                {#each createFiles as attachment, index (`${attachment.name}-${attachment.size}-${index}`)}<div
                                        class="attachment-chip"
                                    >
                                        <span
                                            >📄 {attachment.name}
                                            <small
                                                >{(
                                                    attachment.size / 1024
                                                ).toFixed(0)} KB</small
                                            ></span
                                        ><button
                                            type="button"
                                            aria-label={`Remove ${attachment.name}`}
                                            onclick={() =>
                                                removePostFile(index)}>×</button
                                        >
                                    </div>{/each}
                            </div>{/if}{#if postFileError}<p
                                class="error-text"
                                role="alert"
                            >
                                {postFileError}
                            </p>{/if}
                    </div>
                    {#if isAdmin}<label class="original-toggle"
                            ><input
                                type="checkbox"
                                bind:checked={createOriginal}
                            /><span
                                ><strong>Publish as a Foews Original</strong
                                ><small
                                    >Requires an author, a fact checker, and
                                    three different source links.</small
                                ></span
                            ></label
                        >{#if createOriginal}<div
                                class="editor-select-grid"
                                class:roster-open={verifierMenuOpen}
                            >
                                <label class="profile-field"
                                    ><span class="field-label"
                                        >Credited author</span
                                    ><input
                                        bind:value={createAuthor}
                                        class:invalid={createOriginalSourcesAttempted &&
                                            !createAuthor.trim()}
                                        placeholder="Author name"
                                    />{#if createOriginalSourcesAttempted && !createAuthor.trim()}<span
                                            class="field-help error-text"
                                            >Enter the credited author's name.</span
                                        >{/if}</label
                                >
                                <div class="profile-field">
                                    <span
                                        class="field-label"
                                        id="fact-checker-label"
                                        >Fact checker</span
                                    >
                                    <div
                                        class="custom-select"
                                        use:verifierDropdown={verifierMenuOpen}
                                    >
                                        <button
                                            type="button"
                                            id="fact-checker-trigger"
                                            class="select-trigger"
                                            aria-labelledby="fact-checker-label fact-checker-value"
                                            aria-haspopup="listbox"
                                            aria-controls="fact-checker-options"
                                            aria-expanded={verifierMenuOpen}
                                            onkeydown={verifierKeydown}
                                            onclick={() => {
                                                verifierMenuOpen =
                                                    !verifierMenuOpen;
                                                regionMenuOpen = false;
                                                topicMenuOpen = false;
                                            }}
                                        >
                                            <span id="fact-checker-value"
                                                >{createVerifier}</span
                                            ><span
                                                class="select-chevron"
                                                aria-hidden="true">⌄</span
                                            >
                                        </button>
                                        {#if verifierMenuOpen}
                                            <div
                                                id="fact-checker-options"
                                                class="select-menu"
                                                role="listbox"
                                                aria-labelledby="fact-checker-label"
                                            >
                                                {#each factCheckerRoster as verifier (verifier)}
                                                    <button
                                                        type="button"
                                                        role="option"
                                                        class="region-option"
                                                        class:selected={createVerifier ===
                                                            verifier}
                                                        aria-selected={createVerifier ===
                                                            verifier}
                                                        tabindex={createVerifier ===
                                                        verifier
                                                            ? 0
                                                            : -1}
                                                        onkeydown={verifierKeydown}
                                                        onclick={() => {
                                                            createVerifier =
                                                                verifier;
                                                            verifierMenuOpen = false;
                                                            document
                                                                .getElementById(
                                                                    "fact-checker-trigger",
                                                                )
                                                                ?.focus();
                                                        }}>{verifier}</button
                                                    >
                                                {/each}
                                            </div>
                                        {/if}
                                    </div>
                                </div>
                            </div>
                            <div class="resource-editor">
                                <div class="resource-editor-heading">
                                    <div>
                                        <strong>Fact-check sources</strong><span
                                            >Provide three different source
                                            links for this Original.</span
                                        >
                                    </div>
                                    <span class="section-count">3 SOURCES</span>
                                </div>
                                {#each createOriginalSources as source, index (index)}
                                    <label class="profile-field link-field">
                                        <span class="field-label"
                                            >Link {index + 1}</span
                                        >
                                        <input
                                            type="url"
                                            bind:value={
                                                createOriginalSources[index]
                                            }
                                            class:invalid={originalSourceInvalid(
                                                index,
                                            )}
                                            aria-invalid={originalSourceInvalid(
                                                index,
                                            )}
                                            placeholder="https://example.org/source"
                                        />
                                        {#if originalSourceInvalid(index)}<span
                                                class="field-help error-text"
                                                >Enter a valid source link that
                                                differs from the other sources.</span
                                            >{/if}
                                    </label>
                                {/each}
                            </div>{/if}{/if}
                    <div class="profile-form-footer">
                        <button class="primary-button" type="submit"
                            >Publish post <span aria-hidden="true">→</span
                            ></button
                        >{#if createError}<span class="error-text" role="alert"
                                >{createError}</span
                            >{/if}
                    </div>
                </form>
            </section>
        {:else if selectedRegion === "settings"}
            <section class="hero settings-hero">
                <div class="hero-copy">
                    <span class="eyebrow">PREFERENCES</span>
                    <h1>Settings</h1>
                    <p>Make Foews fit the way you read, browse, and explore.</p>
                </div>
                <div class="settings-summary">
                    <span class="settings-summary-label"
                        >CURRENT APPEARANCE</span
                    ><span class="settings-summary-value"
                        >{darkMode ? "Dark mode" : "Light mode"}</span
                    ><span class="settings-summary-detail"
                        >Changes apply across the interface.</span
                    >
                </div>
            </section>
            <section class="settings-section">
                <div class="section-heading">
                    <div>
                        <h2>Preferences</h2>
                        <p class="section-description">
                            Your reading environment and display options.
                        </p>
                    </div>
                </div>
                <div class="settings-grid">
                    {#each [{ title: "Appearance", description: "Switch between the light and dark colour schemes.", value: darkMode ? "Dark mode enabled" : "Light mode enabled", enabled: darkMode, toggle: () => (darkMode = !darkMode), accent: "accent-appearance" }, { title: "Article excerpts", description: "Show article summaries and expand them while browsing a story.", value: showExcerpts ? "Excerpts visible" : "Excerpts hidden", enabled: showExcerpts, toggle: () => (showExcerpts = !showExcerpts), accent: "accent-reading" }, { title: "Compact capsules", description: "Reduce the spacing around story cards to fit more on screen.", value: compactMode ? "Compact layout enabled" : "Comfortable layout enabled", enabled: compactMode, toggle: () => (compactMode = !compactMode), accent: "accent-layout" }] as setting (setting.title)}<article
                            class="setting-card"
                        >
                            <span class="setting-accent {setting.accent}"
                            ></span>
                            <div class="setting-copy">
                                <h3>{setting.title}</h3>
                                <p>{setting.description}</p>
                                <span class="setting-value"
                                    >{setting.value}</span
                                >
                            </div>
                            <button
                                type="button"
                                class="setting-toggle"
                                class:enabled={setting.enabled}
                                role="switch"
                                aria-checked={setting.enabled}
                                aria-label="Toggle {setting.title}"
                                onclick={setting.toggle}><span></span></button
                            >
                        </article>{/each}
                </div>
            </section>
        {:else if selectedRegion === "originals" && !subscriptionActive && !isAdmin}
            <section class="hero originals-hero">
                <div class="hero-copy">
                    <span class="eyebrow">THE FOEWS EDITORIAL COLLECTION</span>
                    <h1>Foews Originals</h1>
                    <p>
                        Investigations, original interviews, and long-form
                        engineering journalism made by the Foews team.
                    </p>
                    <div class="originals-hero-label">
                        <span>✦</span> Commissioned. Investigated. Fact-checked.
                    </div>
                </div>
                <div class="hero-code">
                    <div class="code-header">
                        <span class="code-dots"
                            ><span></span><span></span><span></span></span
                        ><span class="code-filename"
                            >{helloSnippets.originals.filename}</span
                        ><span class="code-language"
                            >{helloSnippets.originals.language}</span
                        >
                    </div>
                    <pre><code>{helloSnippets.originals.code}</code></pre>
                    <div class="code-footer">
                        <span>FOEWS ORIGINALS</span><span
                            >EDITORIAL COLLECTION</span
                        >
                    </div>
                </div>
            </section>
            <section class="paywall-section">
                <div class="paywall-lock">✍️</div>
                <span class="paywall-eyebrow">EXCLUSIVE TO MEMBERS</span>
                <h2>Stories worth going deeper for.</h2>
                <p>
                    Our Originals are investigations, technical analysis, and
                    interviews commissioned by the Foews editorial team. This
                    collection is reserved for active members.
                </p>
                <div class="paywall-features">
                    <span>✓ Original investigations</span><span
                        >✓ Long-form interviews</span
                    ><span>✓ Editorial fact-check details</span><span
                        >✓ Exclusive technical analysis</span
                    >
                </div>
                <div class="offer-price paywall-price">
                    <strong>€4.99</strong><span>/ month</span>
                </div>
                <button
                    type="button"
                    class="primary-button"
                    onclick={() => selectRegion("account")}
                    >View membership options <span aria-hidden="true">→</span
                    ></button
                >
                <p class="paywall-footnote">
                    Subscription controls are available in your account.
                    Checkout is not connected to a payment provider.
                </p>
            </section>
        {:else if selectedRegion === "originals" && (subscriptionActive || isAdmin)}
            <section class="hero originals-hero">
                <div class="hero-copy">
                    <span class="eyebrow">THE FOEWS EDITORIAL COLLECTION</span>
                    <h1>Foews Originals</h1>
                    <p>
                        Original investigations, interviews, and long-form
                        engineering journalism made by the Foews team.
                    </p>
                    <div class="originals-hero-label">
                        <span>✦</span>{isAdmin
                            ? "Administrator access"
                            : "Membership active"}
                    </div>
                </div>
                <div class="hero-code">
                    <div class="code-header">
                        <span class="code-dots"
                            ><span></span><span></span><span></span></span
                        ><span class="code-filename"
                            >{helloSnippets.originals.filename}</span
                        ><span class="code-language"
                            >{helloSnippets.originals.language}</span
                        >
                    </div>
                    <pre><code>{helloSnippets.originals.code}</code></pre>
                    <div class="code-footer">
                        <span>FOEWS ORIGINALS</span><span
                            >EDITORIAL COLLECTION</span
                        >
                    </div>
                </div>
            </section>
            <section class="coverage-section">
                <div class="section-heading coverage-heading">
                    <div>
                        <h2>Latest Originals</h2>
                        <p class="section-description">
                            Investigations and long-form editorial work.
                        </p>
                    </div>
                    <span class="section-count"
                        >{visibleArticles.length.toString().padStart(2, "0")} stories</span
                    >
                </div>
                <div class="article-stack">
                    {#each visibleArticles as article (article.id)}
                        {@render articleCapsule(article, false)}
                    {/each}
                </div>
            </section>
        {:else}
            <section class="hero">
                <div class="hero-copy">
                    <h1>{regionNames[selectedRegion] ?? "World"}</h1>
                    <p>{currentSlogan}</p>
                </div>
                <div class="hero-code">
                    <div class="code-header">
                        <span class="code-dots"
                            ><span></span><span></span><span></span></span
                        ><span class="code-filename"
                            >{currentHello.filename}</span
                        ><span class="code-language"
                            >{currentHello.language}</span
                        >
                    </div>
                    <pre><code>{currentHello.code}</code></pre>
                    <div class="code-footer">
                        <span>OUTPUT</span><span>Hello, world!</span>
                    </div>
                </div>
            </section>
            <section class="topic-section">
                <div class="section-heading">
                    <h2>Topics</h2>
                    <span class="section-count"
                        >{currentTopics.length.toString().padStart(2, "0")} topics</span
                    >
                </div>
                <div class="topic-grid">
                    {#each currentTopics as topic (topic.id)}<button
                            type="button"
                            class="topic-card"
                            class:selected={activeTopic === topic.id}
                            style={`--topic-gradient:${topic.gradient}`}
                            aria-pressed={activeTopic === topic.id}
                            onclick={() => selectTopic(topic.id)}
                            ><span class="topic-name">{topic.name}</span
                            ></button
                        >{/each}
                </div>
            </section>
            <section class="coverage-section">
                <div class="section-heading coverage-heading">
                    <div>
                        <h2>
                            {activeTopic === "world"
                                ? "Latest coverage"
                                : currentTopics.find(
                                      (topic) => topic.id === activeTopic,
                                  )?.name}
                        </h2>
                        <p class="section-description">
                            Technical reporting, engineering analysis, and
                            research.
                        </p>
                    </div>
                    <span class="section-count"
                        >{visibleArticles.length.toString().padStart(2, "0")} stories</span
                    >
                </div>
                {#if visibleArticles.length}
                    <div class="article-stack">
                        {#each visibleArticles as article (article.id)}
                            {@render articleCapsule(article, false)}
                        {/each}
                    </div>
                {:else}<div class="no-results">
                        <span class="no-results-symbol">\f</span>
                        <h3>No matching coverage</h3>
                        <p>Try another topic.</p>
                        <button
                            type="button"
                            class="primary-button"
                            onclick={() => {
                                activeTopic = "world";
                            }}>Clear filters</button
                        >
                    </div>{/if}
            </section>
        {/if}

        <section class="more-section">
            <div class="section-heading">
                <div>
                    <h2>More to explore</h2>
                    <p class="section-description">
                        {selectedArticle
                            ? "Keep reading across the engineering landscape."
                            : "Keep discovering ideas across the engineering landscape."}
                    </p>
                </div>
            </div>
            {#if selectedArticle && relatedArticles.length > 0}
                <div class="more-article-grid">
                    {#each relatedArticles as article (article.id)}<button
                            type="button"
                            class="more-article-card"
                            style={`--topic-gradient:${article.gradient}`}
                            onclick={() => openArticle(article.id)}
                            ><span class="more-article-label"
                                >{article.topicLabel}</span
                            ><span class="more-article-title"
                                >{article.title}</span
                            ><span class="more-article-meta"
                                >{article.readTime}<span aria-hidden="true"
                                    >→</span
                                ></span
                            ></button
                        >{/each}
                </div>
            {:else}
                <div class="more-grid">
                    {#each relatedTopics as topic (topic.id)}<button
                            type="button"
                            class="more-card"
                            style={`--topic-gradient:${topic.gradient}`}
                            onclick={() => selectTopic(topic.id)}
                            ><span class="more-card-accent"></span><span
                                >{topic.name}</span
                            ></button
                        >{/each}{#if selectedRegion !== "originals" && selectedRegion !== "account" && selectedRegion !== "settings" && selectedRegion !== "create" && !subscriptionActive && !isAdmin}<button
                            type="button"
                            class="more-card originals-more-card"
                            onclick={() => selectRegion("originals")}
                            ><span class="more-card-accent"></span><span
                                >Foews Originals <small>Membership</small></span
                            ></button
                        >{/if}
                </div>
            {/if}
        </section>

        {#if reportArticle !== null}
            <div
                class="modal-backdrop"
                role="presentation"
                onclick={(event) => {
                    if (event.target === event.currentTarget)
                        reportArticle = null;
                }}
            >
                <div
                    class="dialog-card"
                    role="dialog"
                    aria-modal="true"
                    aria-labelledby="report-title"
                    tabindex="-1"
                    use:focusDialog={() => (reportArticle = null)}
                >
                    <button
                        class="dialog-close"
                        type="button"
                        aria-label="Close"
                        onclick={() => (reportArticle = null)}>×</button
                    >
                    {#if reportSubmitted}
                        <div class="report-confirmation">
                            <span class="report-logo">\f</span>
                            <h2 id="report-title">
                                Hi {profileName.trim().split(/\s+/)[0] ||
                                    "there"},
                            </h2>
                            <p>
                                Thank you for looking out for the Foews
                                community. Your anonymous report has been sent
                                to our team for review. We appreciate you taking
                                the time to help keep this space useful and
                                respectful.
                            </p>
                            <button
                                type="button"
                                class="primary-button"
                                onclick={showReportConfirmation}
                                >Close message</button
                            >
                        </div>
                    {:else}
                        <span class="eyebrow">COMMUNITY REVIEW</span>
                        <h2 id="report-title">Report this post</h2>
                        <p class="section-description">
                            Tell the moderation team what needs attention. Your
                            report is anonymous.
                        </p>
                        <form novalidate onsubmit={submitReport}>
                            <label class="profile-field"
                                ><span class="field-label">Your complaint</span
                                ><textarea
                                    bind:value={reportText}
                                    oninput={() => (reportMessage = "")}
                                    class:invalid={!!reportMessage &&
                                        !reportText.trim()}
                                    rows="5"
                                    maxlength="1200"
                                    placeholder="Describe your concern..."
                                ></textarea>{#if reportMessage}<span
                                        class="field-help error-text"
                                        >{reportMessage}</span
                                    >{/if}</label
                            ><button class="primary-button" type="submit"
                                >Send anonymous report</button
                            >
                        </form>
                    {/if}
                </div>
            </div>
        {/if}
        {#if factCheckArticle !== null}
            <div
                class="modal-backdrop"
                role="presentation"
                onclick={(event) => {
                    if (event.target === event.currentTarget) closeFactCheck();
                }}
            >
                <div
                    class="dialog-card fact-dialog"
                    role="dialog"
                    aria-modal="true"
                    aria-labelledby="fact-title"
                    tabindex="-1"
                    use:focusDialog={closeFactCheck}
                >
                    <button
                        class="dialog-close"
                        type="button"
                        aria-label="Close"
                        onclick={closeFactCheck}>×</button
                    >
                    {#if factCheckSubmitted}
                        <div class="report-confirmation">
                            <span class="report-logo">\f</span>
                            <h2 id="fact-title">
                                Thank you, {accountName
                                    .trim()
                                    .split(/\s+/)[0] || "there"}.
                            </h2>
                            <p>
                                Your fact check for “{factCheckedTitle}” is now
                                published. Readers can review your sources and
                                supporting evidence from the post’s verification
                                badge. Thank you for helping the Foews community
                                stay informed.
                            </p>
                            <button
                                type="button"
                                class="primary-button"
                                onclick={closeFactCheck}
                                >Return to coverage</button
                            >
                        </div>
                    {:else}
                        <span class="eyebrow">FACT CHECK</span>
                        <h2 id="fact-title">Submit verification</h2>
                        <p class="section-description">
                            Provide three independent sources. Your account is
                            recorded as the fact checker.
                        </p>
                        <form novalidate onsubmit={submitFactCheck}>
                            {#each factCheckSources as source, index (index)}
                                <label class="profile-field"
                                    ><span class="field-label"
                                        >Source {index + 1} link</span
                                    ><input
                                        type="url"
                                        bind:value={factCheckSources[index]}
                                        oninput={() => (factCheckMessage = "")}
                                        class:invalid={factCheckLinkInvalid(
                                            index,
                                        )}
                                        aria-invalid={factCheckLinkInvalid(
                                            index,
                                        )}
                                        placeholder="https://example.org/source"
                                    />{#if factCheckLinkInvalid(index)}<span
                                            class="field-help error-text"
                                            >Enter a valid source link that
                                            differs from the other two.</span
                                        >{/if}</label
                                >
                            {/each}
                            <div class="resource-editor">
                                <div class="resource-editor-heading">
                                    <div>
                                        <strong>Supporting files</strong><span
                                            >Optional evidence files · up to 10
                                            MB total per post.</span
                                        >
                                    </div>
                                </div>
                                <input
                                    id="fact-files"
                                    class="file-input-hidden"
                                    type="file"
                                    multiple
                                    accept="image/*,.pdf,.txt,.csv,.doc,.docx"
                                    onchange={addFactFiles}
                                />
                                <label
                                    class="file-picker"
                                    class:invalid={factFileError !== ""}
                                    for="fact-files"
                                    ><span aria-hidden="true">📎</span> Add evidence
                                    files</label
                                >
                                {#if factCheckFiles.length}
                                    <div class="attachment-list">
                                        {#each factCheckFiles as attachment, index (`${attachment.name}-${attachment.size}-${index}`)}<div
                                                class="attachment-chip"
                                            >
                                                <span
                                                    >📄 {attachment.name}
                                                    <small
                                                        >{(
                                                            attachment.size /
                                                            1024
                                                        ).toFixed(0)} KB</small
                                                    ></span
                                                ><button
                                                    type="button"
                                                    aria-label={`Remove ${attachment.name}`}
                                                    onclick={() =>
                                                        removeFactFile(index)}
                                                    >×</button
                                                >
                                            </div>{/each}
                                    </div>
                                {/if}
                                {#if factFileError}<p
                                        class="error-bubble"
                                        role="alert"
                                    >
                                        {factFileError}
                                    </p>{/if}
                            </div>
                            {#if factCheckMessage}<p
                                    class="field-help error-text"
                                    role="alert"
                                >
                                    {factCheckMessage}
                                </p>{/if}
                            <button class="primary-button" type="submit"
                                >Submit fact-check</button
                            >
                        </form>
                    {/if}
                </div>
            </div>
        {/if}
        {#if toastMessage}<div class="toast" role="status">
                {toastMessage}
            </div>{/if}{#if downloadError}<div
                class="error-bubble download-error"
                role="alert"
            >
                {downloadError}
            </div>{/if}
    </main>
</div>

<style>
    :global(*) {
        box-sizing: border-box;
    }
    :global(:root) {
        color-scheme: light;
        --page: #faf9f6;
        --surface: #fff;
        --surface-soft: #f4f2ec;
        --border: #e8e6df;
        --border-strong: #d6d3ca;
        --text: #292a25;
        --text-secondary: #62635b;
        --muted: #858379;
        --faint: #a09e95;
        --input-bg: #fff;
        --tooltip-bg: #fff;
        --soft-shadow: rgba(39, 37, 29, 0.035);
        --error: #bd3945;
        --warning: #b57a23;
    }
    :global(:root[data-theme="dark"]) {
        color-scheme: dark;
        --page: #171815;
        --surface: #22231f;
        --surface-soft: #2b2c27;
        --border: #393a33;
        --border-strong: #505147;
        --text: #efeee7;
        --text-secondary: #d2d0c7;
        --muted: #aaa89e;
        --faint: #88877f;
        --input-bg: #22231f;
        --tooltip-bg: #292a25;
        --soft-shadow: rgba(0, 0, 0, 0.16);
        --error: #ff7d85;
        --warning: #e6b653;
    }
    :global(html) {
        min-width: 320px;
        background: var(--page);
        scroll-behavior: smooth;
    }
    :global(body) {
        margin: 0;
        color: var(--text);
        background: var(--page);
        font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo,
            monospace;
        font-size: 13px;
        -webkit-font-smoothing: antialiased;
    }
    :global(button),
    :global(input),
    :global(select),
    :global(textarea) {
        font: inherit;
    }
    :global(button) {
        color: inherit;
    }
    :global(button:focus-visible),
    :global(input:focus-visible),
    :global(select:focus-visible),
    :global(textarea:focus-visible),
    :global([tabindex="0"]:focus-visible) {
        outline: 2px solid #7479dc;
        outline-offset: 3px;
    }
    .app-shell {
        display: grid;
        grid-template-columns: 66px minmax(0, 1fr);
        max-width: 1800px;
        min-height: 100vh;
        margin: 0 auto;
        background: var(--page);
        color: var(--text);
    }
    .sidebar {
        position: sticky;
        top: 0;
        z-index: 30;
        display: flex;
        height: 100vh;
        min-width: 0;
        flex-direction: column;
        align-items: flex-start;
        padding: 24px 0;
        border-right: 1px solid var(--border);
        background: var(--page);
        overflow: visible;
    }
    .brand {
        position: relative;
        z-index: 31;
        display: block;
        flex: 0 0 auto;
        width: 42px;
        height: 42px;
        margin-left: 12px;
        padding: 0;
        overflow: hidden;
        border: 1px solid var(--border);
        border-radius: 14px;
        background: var(--surface);
        color: var(--text);
        cursor: pointer;
        white-space: nowrap;
        transition:
            width 0.3s cubic-bezier(0.2, 0.75, 0.25, 1),
            box-shadow 0.2s ease;
    }
    .brand:hover,
    .brand:focus-visible {
        width: 207px;
        box-shadow: 0 7px 22px var(--soft-shadow);
    }
    .brand-mark {
        position: absolute;
        top: 50%;
        left: 9px;
        display: block;
        width: 24px;
        padding-right: 4px;
        transform: translateY(-50%);
        color: var(--text);
        font-size: 23px;
        font-weight: 800;
        line-height: 1;
        letter-spacing: -4px;
    }
    .brand-equation {
        position: absolute;
        top: 50%;
        left: 44px;
        display: block;
        max-width: 0;
        overflow: hidden;
        transform: translateY(-50%);
        color: var(--text-secondary);
        font-size: 12px;
        letter-spacing: -0.5px;
        opacity: 0;
        white-space: nowrap;
        pointer-events: none;
        transition:
            max-width 0.3s ease,
            opacity 0.2s ease 0.05s;
    }
    .brand:hover .brand-equation,
    .brand:focus-visible .brand-equation {
        max-width: 150px;
        opacity: 1;
    }
    .brand-equation em {
        color: var(--text);
        font-family: Georgia, "Times New Roman", serif;
        font-size: 14px;
        font-weight: 500;
    }
    .region-nav {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 9px;
        margin-top: 30px;
        margin-left: 5px;
        padding: 12px 7px;
        border: 1px solid var(--border);
        border-radius: 28px;
        background: var(--surface);
    }
    .region-button {
        display: grid;
        width: 38px;
        height: 38px;
        place-items: center;
        padding: 0;
        border: 1px solid transparent;
        border-radius: 50%;
        background: transparent;
        cursor: pointer;
        transition:
            background 0.18s ease,
            border-color 0.18s ease,
            transform 0.18s ease;
    }
    .region-button:hover {
        background: var(--surface-soft);
        transform: scale(1.05);
    }
    .region-button.active {
        border-color: var(--border-strong);
        background: var(--surface-soft);
    }
    .region-icon {
        font-family: "Apple Color Emoji", "Segoe UI Emoji", sans-serif;
        font-size: 19px;
        line-height: 1;
    }
    .workspace {
        width: 100%;
        min-width: 0;
        max-width: 1280px;
        margin: 0 auto;
        padding: 28px clamp(20px, 4.5vw, 64px) 80px;
    }
    .topbar {
        display: flex;
        min-height: 46px;
        align-items: center;
        justify-content: space-between;
        gap: 20px;
        margin-bottom: 35px;
    }
    .topbar-left,
    .topbar-right {
        display: flex;
        min-width: 0;
        align-items: center;
        gap: 11px;
    }
    .topbar-right {
        justify-content: flex-end;
        gap: 16px;
    }
    .region-name {
        min-width: 0;
        color: var(--text);
        font-size: 13px;
        font-weight: 700;
        letter-spacing: -0.4px;
        overflow-wrap: anywhere;
    }
    .article-breadcrumb {
        display: -webkit-box;
        overflow: hidden;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 2;
        line-clamp: 2;
    }
    .context-separator {
        color: var(--faint);
        font-size: 12px;
    }
    .context-topic {
        color: var(--muted);
        font-size: 11px;
    }
    .back-link {
        display: grid;
        width: 29px;
        height: 29px;
        flex-shrink: 0;
        place-items: center;
        border: 1px solid var(--border);
        border-radius: 9px;
        background: var(--surface);
        color: var(--text);
        font-size: 16px;
        cursor: pointer;
    }

    .wordmark-small {
        flex-shrink: 0;
        color: var(--text);
        font-size: 10px;
        font-weight: 800;
        letter-spacing: 0.12em;
    }
    .hero {
        position: relative;
        display: flex;
        min-height: 235px;
        align-items: center;
        justify-content: space-between;
        gap: 25px;
        overflow: hidden;
        padding: 31px 34px;
        border: 1px solid var(--border);
        border-radius: 22px;
        background: linear-gradient(
            120deg,
            #e5e6fb 0%,
            #d9eeea 51%,
            #f8e4d4 100%
        );
        color: #292b36;
    }
    .hero-copy {
        position: relative;
        z-index: 2;
        flex: 1 1 43%;
        min-width: 0;
    }
    .hero h1 {
        margin: 0;
        color: #292b36;
        font-size: clamp(30px, 4vw, 44px);
        font-weight: 700;
        line-height: 1.12;
        letter-spacing: -2.5px;
        overflow-wrap: anywhere;
    }
    .hero-copy p {
        max-width: 410px;
        margin: 15px 0 0;
        color: #61655f;
        font-size: 11px;
        line-height: 1.9;
    }
    .hero-code {
        position: relative;
        z-index: 2;
        flex: 0 1 49%;
        min-width: 0;
        padding: 14px 15px 11px;
        border: 1px solid rgba(255, 255, 255, 0.28);
        border-radius: 13px;
        background: rgba(34, 36, 47, 0.93);
        box-shadow: 0 12px 25px rgba(38, 43, 59, 0.12);
        color: #e8e9ed;
    }
    .code-header {
        display: flex;
        align-items: center;
        gap: 10px;
        padding-bottom: 12px;
        border-bottom: 1px solid rgba(255, 255, 255, 0.12);
    }
    .code-dots {
        display: flex;
        flex-shrink: 0;
        gap: 4px;
    }
    .code-dots span {
        width: 6px;
        height: 6px;
        border-radius: 50%;
        background: #ee8b82;
    }
    .code-dots span:nth-child(2) {
        background: #e9c46a;
    }
    .code-dots span:nth-child(3) {
        background: #79c7a2;
    }
    .code-filename {
        min-width: 0;
        color: #b9bac7;
        font-size: 9px;
        overflow-wrap: anywhere;
    }
    .code-language {
        margin-left: auto;
        color: #b4d7cb;
        font-size: 9px;
        white-space: nowrap;
    }
    .hero-code pre {
        min-height: 91px;
        max-width: 100%;
        margin: 13px 0 10px;
        overflow: auto;
        color: #e4e6ee;
        font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo,
            monospace;
        font-size: 10px;
        line-height: 1.8;
        tab-size: 4;
    }
    .hero-code code {
        font-family: inherit;
        white-space: pre;
    }
    .code-footer {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 8px;
        padding-top: 9px;
        border-top: 1px solid rgba(255, 255, 255, 0.12);
        color: #b0caba;
        font-size: 8px;
        letter-spacing: 0.3px;
    }
    .eyebrow {
        display: inline-block;
        margin-bottom: 14px;
        color: #656960;
        font-size: 9px;
        letter-spacing: 0.8px;
        text-transform: uppercase;
    }
    .originals-hero {
        background: linear-gradient(
            120deg,
            #f4e1fc 0%,
            #e2e4fb 45%,
            #d6f0e6 100%
        );
    }
    .originals-hero .hero-copy p {
        color: #52546a;
    }
    .originals-hero .eyebrow {
        color: #635684;
    }
    .originals-hero-label {
        display: inline-flex;
        flex-wrap: wrap;
        align-items: center;
        gap: 8px;
        margin-top: 17px;
        padding: 9px 11px;
        border: 1px solid rgba(255, 255, 255, 0.8);
        border-radius: 9px;
        background: rgba(255, 255, 255, 0.42);
        color: #4b4868;
        font-size: 9px;
    }
    .originals-hero-label span {
        color: #8053bc;
        font-size: 13px;
    }
    .account-hero,
    .settings-hero {
        min-height: 220px;
    }
    .account-hero .hero-copy,
    .settings-hero .hero-copy {
        flex: 1 1 55%;
    }
    .profile-summary,
    .settings-summary {
        display: flex;
        flex: 0 1 35%;
        min-width: 160px;
        flex-direction: column;
        align-items: flex-start;
        gap: 10px;
        padding: 17px;
        border: 1px solid rgba(255, 255, 255, 0.55);
        border-radius: 15px;
        background: rgba(255, 255, 255, 0.38);
        color: #353748;
    }
    .profile-initial {
        display: grid;
        width: 46px;
        height: 46px;
        place-items: center;
        border: 1px solid rgba(255, 255, 255, 0.75);
        border-radius: 14px;
        background: rgba(255, 255, 255, 0.6);
        font-size: 22px;
        font-weight: 800;
    }
    .profile-summary-label,
    .settings-summary-label {
        color: #71736d;
        font-size: 8px;
        letter-spacing: 0.6px;
    }
    .profile-summary-name,
    .settings-summary-value {
        color: #292b36;
        font-size: 14px;
        font-weight: 700;
        overflow-wrap: anywhere;
    }
    .subscription-badge {
        padding: 6px 8px;
        border: 1px solid rgba(88, 85, 109, 0.2);
        border-radius: 6px;
        color: #686579;
        font-size: 8px;
        letter-spacing: 0.3px;
    }
    .subscription-badge.subscription-active {
        border-color: #9cc7b0;
        background: rgba(220, 244, 230, 0.7);
        color: #276648;
    }
    .settings-summary-detail,
    .role-note {
        color: #71736d;
        font-size: 9px;
        line-height: 1.7;
    }
    .topic-section,
    .coverage-section,
    .more-section,
    .profile-section,
    .settings-section {
        margin-top: 34px;
    }
    .section-heading {
        display: flex;
        align-items: baseline;
        justify-content: space-between;
        gap: 15px;
        margin-bottom: 16px;
    }
    .section-heading h2 {
        margin: 0;
        color: var(--text);
        font-size: 14px;
        font-weight: 700;
        letter-spacing: -0.45px;
    }
    .section-count {
        color: var(--faint);
        font-size: 10px;
        white-space: nowrap;
    }
    .topic-grid {
        display: grid;
        grid-template-columns: repeat(3, minmax(0, 1fr));
        gap: 12px;
    }
    .topic-card {
        position: relative;
        display: flex;
        min-width: 0;
        min-height: 77px;
        align-items: flex-end;
        overflow: hidden;
        padding: 15px 16px;
        border: 1px solid rgba(255, 255, 255, 0.65);
        border-radius: 14px;
        background: var(--topic-gradient);
        color: #fff;
        text-align: left;
        cursor: pointer;
        transition:
            transform 0.18s ease,
            box-shadow 0.18s ease,
            filter 0.18s ease;
    }
    .topic-card::before {
        position: absolute;
        inset: 0;
        background: linear-gradient(
            145deg,
            rgba(255, 255, 255, 0.17),
            transparent 65%
        );
        content: "";
        pointer-events: none;
    }
    .topic-card:hover {
        transform: translateY(-3px);
        box-shadow: 0 8px 22px rgba(38, 38, 38, 0.1);
        filter: saturate(1.08);
    }
    .topic-card.selected {
        outline: 2px solid var(--text);
        outline-offset: 2px;
    }
    .topic-name {
        position: relative;
        z-index: 1;
        font-size: 11px;
        font-weight: 700;
        overflow-wrap: anywhere;
    }
    .coverage-heading {
        align-items: flex-end;
        margin-bottom: 19px;
    }
    .section-description {
        margin: 7px 0 0;
        color: var(--muted);
        font-size: 10px;
        line-height: 1.7;
    }
    .article-stack {
        display: flex;
        min-width: 0;
        flex-direction: column;
        gap: 12px;
    }
    .capsule-wrap {
        position: relative;
        min-width: 0;
        overflow: visible;
        border-radius: 17px;
    }
    .article-capsule {
        position: relative;
        display: grid;
        grid-template-columns: 145px minmax(0, 1fr);
        width: 100%;
        min-width: 0;
        overflow: visible;
        border: 1px solid var(--border);
        border-radius: 17px;
        background: var(--surface);
        cursor: pointer;
        touch-action: pan-y;
        transition:
            width 0.22s ease,
            border-color 0.2s ease,
            box-shadow 0.2s ease;
    }
    .article-capsule:hover {
        border-color: var(--border-strong);
        box-shadow: 0 7px 22px var(--soft-shadow);
    }
    .article-capsule.swiped {
        width: calc(100% - var(--capsule-rail-width));
    }
    .capsule-actions {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        display: flex;
        width: var(--capsule-rail-width);
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 4px;
        padding: 8px 4px;
        border: 1px solid var(--border);
        border-radius: 0 17px 17px 0;
        background: var(--surface-soft);
        opacity: 0;
        visibility: hidden;
        pointer-events: none;
        transition:
            opacity 0.15s ease,
            visibility 0.15s ease;
    }
    .capsule-actions button {
        display: grid;
        width: 38px;
        height: 38px;
        min-height: 24px;
        max-height: 38px;
        flex: 0 1 38px;
        place-items: center;
        padding: 0;
        border: 1px solid transparent;
        border-radius: 50%;
        background: transparent;
        font-size: 18px;
        cursor: pointer;
    }
    .capsule-actions button:hover {
        background: var(--surface);
    }
    .article-visual {
        position: relative;
        display: flex;
        min-height: 160px;
        flex-direction: column;
        align-items: flex-start;
        justify-content: space-between;
        gap: 8px;
        overflow: hidden;
        padding: 14px;
        border-radius: 16px 0 0 16px;
        background: var(--article-gradient);
        color: #fff;
    }
    .article-topic,
    .article-region {
        position: relative;
        z-index: 1;
        max-width: 100%;
        padding: 6px 8px;
        border: 1px solid rgba(255, 255, 255, 0.45);
        border-radius: 6px;
        background: rgba(255, 255, 255, 0.15);
        backdrop-filter: blur(6px);
        font-size: 9px;
        line-height: 1.4;
        overflow-wrap: anywhere;
    }
    .article-region {
        font-size: 8px;
    }
    .article-main {
        display: flex;
        min-width: 0;
        flex-direction: column;
        align-items: stretch;
        padding: 16px 18px 13px;
    }
    .article-meta {
        display: flex;
        align-items: center;
        flex-wrap: wrap;
        gap: 7px;
        color: var(--muted);
        font-size: 9px;
        line-height: 1.6;
    }
    .meta-dot {
        color: var(--faint);
    }
    .article-main h3 {
        margin: 11px 0 8px;
        color: var(--text);
        font-size: 13px;
        font-weight: 700;
        line-height: 1.65;
        letter-spacing: -0.35px;
        overflow-wrap: anywhere;
    }
    .article-excerpt {
        margin: 0;
        color: var(--text-secondary);
        font-size: 10px;
        line-height: 1.85;
        overflow-wrap: anywhere;
    }
    .article-excerpt-collapsed {
        display: -webkit-box;
        overflow: hidden;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 2;
        line-clamp: 2;
    }
    .article-footer {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 12px;
        margin-top: auto;
        padding-top: 13px;
    }
    .article-team {
        min-width: 0;
        color: var(--muted);
        font-size: 9px;
        overflow-wrap: anywhere;
    }
    .article-actions {
        position: relative;
        z-index: 3;
        display: flex;
        align-items: center;
        gap: 13px;
    }
    .like-button {
        display: inline-flex;
        min-height: 27px;
        align-items: center;
        gap: 6px;
        padding: 3px;
        border: 0;
        border-radius: 5px;
        background: transparent;
        color: var(--muted);
        cursor: pointer;
    }
    .heart-icon {
        font-family: Arial, sans-serif;
        font-size: 19px;
        line-height: 1;
        transition:
            transform 0.15s ease,
            color 0.15s ease;
    }
    .like-button:hover .heart-icon {
        transform: scale(1.16);
        color: #e75b78;
    }
    .like-button.liked .heart-icon {
        color: #e75b78;
    }
    .like-count {
        max-width: 0;
        overflow: hidden;
        font-size: 9px;
        opacity: 0;
        white-space: nowrap;
        transition:
            max-width 0.2s ease,
            opacity 0.15s ease;
    }
    .like-button:hover .like-count,
    .like-button:focus-visible .like-count,
    .like-count.visible {
        max-width: 75px;
        opacity: 1;
    }
    .verification-wrap {
        position: relative;
        display: inline-flex;
        align-items: center;
    }
    .verification-icon {
        display: grid;
        width: 19px;
        height: 19px;
        place-items: center;
        padding: 0;
        border: 1px solid var(--border-strong);
        border-radius: 50%;
        background: var(--surface);
        color: var(--faint);
        font-size: 11px;
        font-weight: 700;
        cursor: help;
    }
    .verification-icon.verified {
        background: var(--surface-soft);
        color: var(--text-secondary);
    }
    .verification-tooltip {
        position: absolute;
        right: -4px;
        bottom: calc(100% + 10px);
        z-index: 20;
        display: flex;
        width: 225px;
        flex-direction: column;
        gap: 7px;
        padding: 13px;
        border: 1px solid var(--border);
        border-radius: 10px;
        background: var(--tooltip-bg);
        box-shadow: 0 10px 35px var(--soft-shadow);
        color: var(--muted);
        font-size: 9px;
        line-height: 1.7;
        opacity: 0;
        visibility: hidden;
        transform: translateY(4px);
        transition:
            opacity 0.15s ease,
            transform 0.15s ease,
            visibility 0.15s ease;
        pointer-events: none;
    }
    .verification-tooltip strong {
        color: var(--text);
        font-size: 10px;
    }
    .verification-wrap:hover .verification-tooltip,
    .verification-wrap:focus-within .verification-tooltip {
        opacity: 1;
        visibility: visible;
        transform: translateY(0);
    }
    .reader-page {
        max-width: 850px;
        margin: 0 auto;
        padding: 22px 0 20px;
    }
    .reader-kicker {
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        gap: 9px;
        margin-bottom: 22px;
        color: var(--muted);
        font-size: 10px;
    }
    .reader-title {
        max-width: 780px;
        margin: 0;
        color: var(--text);
        font-size: clamp(28px, 4vw, 43px);
        font-weight: 700;
        letter-spacing: -1.8px;
        line-height: 1.3;
        overflow-wrap: anywhere;
    }
    .reader-deck {
        max-width: 730px;
        margin: 21px 0 27px;
        color: var(--text-secondary);
        font-size: 13px;
        line-height: 1.95;
        overflow-wrap: anywhere;
    }
    .reader-byline {
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-wrap: wrap;
        gap: 18px;
        padding: 19px 0;
        border-top: 1px solid var(--border);
        border-bottom: 1px solid var(--border);
    }
    .reader-author {
        display: flex;
        align-items: center;
        gap: 11px;
    }
    .author-avatar {
        display: grid;
        width: 39px;
        height: 39px;
        flex-shrink: 0;
        place-items: center;
        border: 1px solid var(--border);
        border-radius: 13px;
        background: var(--surface-soft);
        color: var(--text);
        font-size: 15px;
        font-weight: 700;
    }
    .reader-author div {
        display: flex;
        flex-direction: column;
        gap: 5px;
    }
    .reader-author strong {
        color: var(--text);
        font-size: 10px;
    }
    .reader-author div span,
    .reader-publication span {
        color: var(--muted);
        font-size: 9px;
    }
    .reader-publication {
        display: flex;
        flex-wrap: wrap;
        gap: 12px;
    }
    .reader-facts {
        display: grid;
        grid-template-columns: repeat(3, minmax(0, 1fr));
        gap: 11px;
        margin-top: 17px;
    }
    .reader-fact {
        display: flex;
        min-width: 0;
        min-height: 87px;
        flex-direction: column;
        align-items: flex-start;
        gap: 8px;
        padding: 13px;
        border: 1px solid var(--border);
        border-radius: 12px;
        background: var(--surface);
    }
    .fact-label {
        color: var(--faint);
        font-size: 8px;
        letter-spacing: 0.5px;
    }
    .reader-fact strong {
        color: var(--text);
        font-size: 10px;
        line-height: 1.6;
        overflow-wrap: anywhere;
    }
    .reader-fact > span:not(.fact-label) {
        color: var(--muted);
        font-size: 8px;
        line-height: 1.6;
    }
    .reader-fact .verified-text,
    .success-text {
        color: #39805b;
    }
    .reader-fact .pending-text {
        color: #a17a56;
    }
    .reader-rule {
        height: 1px;
        margin: 28px 0;
        background: var(--border);
    }
    .reader-content {
        max-width: 720px;
        margin: 0 auto;
        color: var(--text);
        font-size: 12px;
        line-height: 2.15;
        overflow-wrap: anywhere;
    }
    .reader-content p {
        margin: 0 0 24px;
    }
    .reader-end {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
        margin: 43px 0 15px;
        color: var(--muted);
        font-size: 9px;
    }
    .reader-end-mark {
        color: var(--text);
        font-size: 19px;
        font-weight: 800;
        letter-spacing: -3px;
    }
    .profile-form,
    .editor-form {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 12px;
        padding: 18px;
        border: 1px solid var(--border);
        border-radius: 17px;
        background: var(--surface);
    }
    .profile-field {
        display: flex;
        min-width: 0;
        flex-direction: column;
        gap: 9px;
        padding: 13px;
        border: 1px solid var(--border);
        border-radius: 12px;
        background: var(--surface);
    }
    .field-label {
        color: var(--text);
        font-size: 10px;
        font-weight: 700;
    }
    .field-inline-note {
        margin-left: 6px;
        color: var(--muted);
        font-size: 8px;
        font-weight: 400;
    }
    .profile-field input,
    .profile-field textarea {
        width: 100%;
        min-width: 0;
        padding: 11px 12px;
        border: 1px solid var(--border);
        border-radius: 8px;
        background: var(--input-bg);
        color: var(--text);
        font-size: 10px;
    }
    .profile-field textarea {
        resize: vertical;
        line-height: 1.8;
    }
    .profile-field input::placeholder,
    .profile-field textarea::placeholder {
        color: var(--faint);
    }
    .field-help {
        color: var(--muted);
        font-size: 9px;
        line-height: 1.7;
    }
    .profile-field input.invalid,
    .profile-field textarea.invalid,
    .invalid {
        border-color: var(--error) !important;
        box-shadow: 0 0 0 1px color-mix(in srgb, var(--error) 28%, transparent);
    }
    .profile-field input.warning {
        border-color: var(--warning);
        box-shadow: 0 0 0 1px
            color-mix(in srgb, var(--warning) 24%, transparent);
    }
    .warning-text {
        color: var(--warning);
    }
    .error-text {
        color: var(--error);
    }
    .phone-field {
        grid-column: 1/-1;
    }
    .phone-inputs {
        display: grid;
        grid-template-columns: minmax(85px, 0.27fr) minmax(0, 1fr);
        gap: 9px;
    }
    .profile-form-footer {
        grid-column: 1/-1;
        display: flex;
        align-items: center;
        flex-wrap: wrap;
        gap: 13px;
        padding-top: 4px;
    }
    .primary-button,
    .secondary-button {
        display: inline-flex;
        min-height: 38px;
        align-items: center;
        justify-content: center;
        gap: 9px;
        padding: 10px 15px;
        border: 1px solid var(--text);
        border-radius: 9px;
        background: var(--text);
        color: var(--page);
        font-size: 10px;
        font-weight: 700;
        cursor: pointer;
        transition:
            opacity 0.15s ease,
            transform 0.15s ease;
    }
    .primary-button:hover,
    .secondary-button:hover {
        opacity: 0.86;
        transform: translateY(-1px);
    }
    .primary-button:disabled {
        opacity: 0.45;
        cursor: not-allowed;
        transform: none;
    }
    .secondary-button {
        border-color: var(--border-strong);
        background: var(--surface);
        color: var(--text);
    }
    .save-message {
        color: #39805b;
        font-size: 10px;
    }
    .inline-message {
        margin: 12px 2px 0;
        color: var(--text-secondary);
        font-size: 10px;
        line-height: 1.8;
    }
    .password-summary {
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-wrap: wrap;
        gap: 13px;
        padding: 18px;
        border: 1px solid var(--border);
        border-radius: 15px;
        background: var(--surface);
        color: var(--text-secondary);
        font-size: 10px;
    }
    .password-input {
        position: relative;
        display: flex;
        align-items: center;
    }
    .password-input input {
        padding-right: 43px;
    }
    .reveal-button {
        position: absolute;
        right: 8px;
        display: grid;
        width: 29px;
        height: 29px;
        place-items: center;
        border: 0;
        border-radius: 7px;
        background: transparent;
        cursor: pointer;
    }
    .reveal-button:hover {
        background: var(--surface-soft);
    }
    .password-rules {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 6px 12px;
        color: var(--muted);
        font-size: 8px;
    }
    .password-rules .rule-met {
        color: #39805b;
    }
    .password-form {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
    .subscription-offer,
    .subscription-active-card {
        padding: 22px;
        border: 1px solid var(--border);
        border-radius: 18px;
        background: var(--surface);
    }
    .offer-topline {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
        gap: 10px;
    }
    .offer-kicker {
        color: #8a73a6;
        font-size: 9px;
        font-weight: 700;
        letter-spacing: 0.7px;
    }
    .offer-label {
        padding: 6px 8px;
        border: 1px solid var(--border);
        border-radius: 6px;
        color: var(--muted);
        font-size: 8px;
    }
    .subscription-offer > h3 {
        margin: 22px 0 10px;
        color: var(--text);
        font-size: 22px;
        letter-spacing: -0.8px;
    }
    .subscription-offer > p {
        max-width: 700px;
        color: var(--text-secondary);
        font-size: 10px;
        line-height: 1.9;
    }
    .offer-benefits {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 12px;
        margin: 24px 0;
    }
    .offer-benefits div {
        display: flex;
        gap: 9px;
        color: var(--text-secondary);
        font-size: 10px;
        line-height: 1.7;
    }
    .offer-benefits div span {
        color: #39805b;
    }
    .offer-price {
        display: flex;
        align-items: baseline;
        flex-wrap: wrap;
        gap: 9px;
        margin-top: 22px;
    }
    .offer-price strong {
        color: var(--text);
        font-size: 29px;
        letter-spacing: -1px;
    }
    .offer-price span {
        color: var(--muted);
        font-size: 9px;
    }
    .billing-form {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 11px;
        margin-top: 23px;
        padding-top: 22px;
        border-top: 1px solid var(--border);
    }
    .billing-heading {
        grid-column: 1/-1;
        display: flex;
        justify-content: space-between;
        align-items: baseline;
        flex-wrap: wrap;
        gap: 10px;
    }
    .billing-heading h4 {
        margin: 0;
        color: var(--text);
        font-size: 12px;
    }
    .billing-heading span {
        color: var(--muted);
        font-size: 8px;
        letter-spacing: 0.5px;
    }
    .card-number-field,
    .billing-row,
    .subscribe-button,
    .billing-disclaimer {
        grid-column: 1/-1;
    }
    .billing-row {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 11px;
    }
    .subscribe-button {
        width: 100%;
        min-height: 43px;
    }
    .billing-disclaimer {
        margin: 0;
        color: var(--muted);
        font-size: 8px;
        line-height: 1.8;
    }
    .subscription-active-card {
        border-color: #c9dfd0;
    }
    .subscription-card-main {
        display: flex;
        align-items: flex-start;
        gap: 14px;
    }
    .subscription-symbol {
        display: grid;
        width: 45px;
        height: 45px;
        flex-shrink: 0;
        place-items: center;
        border: 1px solid #c9dfd0;
        border-radius: 13px;
        background: #edf8f0;
        font-size: 19px;
    }
    .subscription-status-label {
        color: #39805b;
        font-size: 8px;
        letter-spacing: 0.5px;
    }
    .subscription-card-main h3 {
        margin: 8px 0;
        color: var(--text);
        font-size: 15px;
    }
    .subscription-card-main p {
        margin: 0;
        color: var(--text-secondary);
        font-size: 10px;
        line-height: 1.8;
    }
    .subscription-details {
        display: grid;
        grid-template-columns: repeat(3, minmax(0, 1fr));
        gap: 12px;
        margin-top: 22px;
        padding: 17px 0;
        border-top: 1px solid var(--border);
        border-bottom: 1px solid var(--border);
    }
    .subscription-details div {
        display: flex;
        min-width: 0;
        flex-direction: column;
        gap: 7px;
    }
    .subscription-details span {
        color: var(--muted);
        font-size: 8px;
    }
    .subscription-details strong {
        color: var(--text);
        font-size: 9px;
        line-height: 1.7;
        overflow-wrap: anywhere;
    }
    .subscription-actions {
        display: flex;
        flex-wrap: wrap;
        gap: 10px;
        margin-top: 17px;
    }
    .subscription-message {
        margin: 12px 2px 0;
        color: var(--text-secondary);
        font-size: 10px;
        line-height: 1.8;
    }
    .editor-form {
        display: flex;
        flex-direction: column;
    }
    .editor-form > .profile-field {
        width: 100%;
    }
    .editor-select-grid {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 12px;
    }
    .original-toggle {
        display: flex;
        align-items: flex-start;
        gap: 11px;
        padding: 14px;
        border: 1px solid var(--border);
        border-radius: 12px;
        background: var(--surface-soft);
    }
    .original-toggle input {
        margin-top: 2px;
        accent-color: #6864c9;
    }
    .original-toggle span {
        display: flex;
        flex-direction: column;
        gap: 5px;
    }
    .original-toggle strong {
        color: var(--text);
        font-size: 10px;
    }
    .original-toggle small {
        color: var(--muted);
        font-size: 9px;
        line-height: 1.7;
    }
    .settings-grid {
        display: flex;
        flex-direction: column;
        gap: 11px;
    }
    .setting-card {
        display: grid;
        grid-template-columns: 7px minmax(0, 1fr) auto;
        align-items: center;
        gap: 17px;
        min-width: 0;
        padding: 18px;
        border: 1px solid var(--border);
        border-radius: 16px;
        background: var(--surface);
        transition:
            border-color 0.18s ease,
            box-shadow 0.18s ease;
    }
    .setting-card:hover {
        border-color: var(--border-strong);
        box-shadow: 0 6px 18px var(--soft-shadow);
    }
    .setting-accent {
        width: 6px;
        height: 48px;
        border-radius: 8px;
        background: linear-gradient(180deg, #8b7ee8, #e7a3cc);
    }
    .accent-reading {
        background: linear-gradient(180deg, #e9a34b, #eb688c);
    }
    .accent-layout {
        background: linear-gradient(180deg, #39b79b, #6f8fe8);
    }
    .setting-copy {
        min-width: 0;
    }
    .setting-copy h3 {
        margin: 0;
        color: var(--text);
        font-size: 12px;
        font-weight: 700;
    }
    .setting-copy p {
        max-width: 520px;
        margin: 7px 0;
        color: var(--text-secondary);
        font-size: 10px;
        line-height: 1.8;
    }
    .setting-value {
        color: var(--muted);
        font-size: 9px;
    }
    .setting-toggle {
        position: relative;
        display: flex;
        width: 44px;
        height: 25px;
        align-items: center;
        flex-shrink: 0;
        padding: 3px;
        border: 1px solid var(--border-strong);
        border-radius: 20px;
        background: var(--surface-soft);
        cursor: pointer;
        transition:
            background 0.2s ease,
            border-color 0.2s ease;
    }
    .setting-toggle span {
        display: block;
        width: 17px;
        height: 17px;
        border-radius: 50%;
        background: #fff;
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.17);
        transform: translateX(0);
        transition: transform 0.2s ease;
    }
    .setting-toggle.enabled {
        border-color: #6864c9;
        background: #6864c9;
    }
    .setting-toggle.enabled span {
        transform: translateX(18px);
    }
    .paywall-section {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 34px;
        padding: 43px 25px;
        border: 1px solid var(--border);
        border-radius: 21px;
        background: var(--surface);
        text-align: center;
    }
    .paywall-lock {
        display: grid;
        width: 63px;
        height: 63px;
        place-items: center;
        border: 1px solid #e0d5eb;
        border-radius: 20px;
        background: #f6effb;
        color: #7957a2;
        font-size: 27px;
    }
    .paywall-eyebrow {
        margin-top: 21px;
        color: #8a73a6;
        font-size: 9px;
        font-weight: 700;
        letter-spacing: 1px;
    }
    .paywall-section h2 {
        max-width: 550px;
        margin: 14px 0 0;
        color: var(--text);
        font-size: clamp(21px, 3vw, 31px);
        letter-spacing: -1px;
        line-height: 1.4;
    }
    .paywall-section > p:not(.paywall-footnote) {
        max-width: 580px;
        margin: 15px 0 0;
        color: var(--text-secondary);
        font-size: 11px;
        line-height: 2;
    }
    .paywall-features {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 12px 22px;
        width: min(100%, 530px);
        margin: 25px 0 7px;
        text-align: left;
    }
    .paywall-features span {
        color: var(--text-secondary);
        font-size: 10px;
        line-height: 1.7;
    }
    .paywall-section > .primary-button {
        margin-top: 17px;
    }
    .paywall-footnote {
        max-width: 440px;
        margin: 14px 0 0;
        color: var(--muted);
        font-size: 8px;
        line-height: 1.8;
    }
    .more-section {
        padding-top: 5px;
    }
    .more-grid,
    .more-article-grid {
        display: grid;
        grid-template-columns: repeat(3, minmax(0, 1fr));
        gap: 12px;
    }
    .more-card {
        display: flex;
        min-width: 0;
        min-height: 83px;
        align-items: center;
        gap: 12px;
        padding: 14px;
        border: 1px solid var(--border);
        border-radius: 13px;
        background: var(--surface);
        color: var(--text);
        text-align: left;
        cursor: pointer;
        font-size: 11px;
        font-weight: 700;
        transition:
            border-color 0.18s ease,
            transform 0.18s ease;
    }
    .more-card:hover,
    .more-article-card:hover {
        border-color: var(--border-strong);
        transform: translateY(-2px);
    }
    .more-card-accent {
        width: 5px;
        height: 37px;
        flex: 0 0 5px;
        border-radius: 8px;
        background: var(--topic-gradient);
    }
    .originals-more-card .more-card-accent {
        background: linear-gradient(180deg, #7209b7, #f9c74f);
    }
    .originals-more-card small {
        display: block;
        margin-top: 6px;
        color: var(--muted);
        font-size: 8px;
        font-weight: 400;
    }
    .more-article-card {
        display: flex;
        min-width: 0;
        min-height: 132px;
        flex-direction: column;
        align-items: flex-start;
        gap: 12px;
        padding: 16px;
        border: 1px solid var(--border);
        border-radius: 14px;
        background: var(--surface);
        color: var(--text);
        text-align: left;
        cursor: pointer;
        transition:
            border-color 0.18s ease,
            transform 0.18s ease;
    }
    .more-article-label {
        display: inline-block;
        padding: 6px 8px;
        border-radius: 6px;
        background: var(--topic-gradient);
        color: #fff;
        font-size: 8px;
    }
    .more-article-title {
        color: var(--text);
        font-size: 10px;
        font-weight: 700;
        line-height: 1.8;
    }
    .more-article-meta {
        display: flex;
        width: 100%;
        align-items: center;
        justify-content: space-between;
        gap: 8px;
        margin-top: auto;
        color: var(--muted);
        font-size: 9px;
    }
    .no-results {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 60px 20px;
        border: 1px solid var(--border);
        border-radius: 18px;
        background: var(--surface);
        text-align: center;
    }
    .no-results-symbol {
        color: var(--muted);
        font-size: 32px;
        font-weight: 800;
    }
    .no-results h3 {
        margin: 16px 0 7px;
        color: var(--text);
        font-size: 13px;
    }
    .no-results p {
        color: var(--muted);
        font-size: 10px;
    }
    .no-results .primary-button {
        margin-top: 12px;
    }
    .modal-backdrop {
        position: fixed;
        inset: 0;
        z-index: 100;
        display: grid;
        place-items: center;
        overflow: auto;
        padding: 20px;
        background: rgba(20, 21, 19, 0.45);
        backdrop-filter: blur(3px);
    }
    .dialog-card {
        position: relative;
        width: min(100%, 520px);
        max-height: min(90vh, 850px);
        overflow: auto;
        padding: 27px;
        border: 1px solid var(--border);
        border-radius: 18px;
        background: var(--surface);
        box-shadow: 0 18px 60px rgba(0, 0, 0, 0.18);
    }
    .dialog-card h2 {
        margin: 0;
        color: var(--text);
        font-size: 21px;
        letter-spacing: -0.8px;
    }
    .dialog-card > .section-description {
        margin: 9px 0 18px;
    }
    .dialog-card form {
        display: flex;
        flex-direction: column;
        gap: 10px;
    }
    .dialog-close {
        position: absolute;
        top: 13px;
        right: 13px;
        display: grid;
        width: 31px;
        height: 31px;
        place-items: center;
        border: 1px solid var(--border);
        border-radius: 9px;
        background: var(--surface);
        font-size: 19px;
        cursor: pointer;
    }
    .toast {
        position: fixed;
        right: 22px;
        bottom: 22px;
        z-index: 120;
        max-width: min(440px, calc(100vw - 44px));
        padding: 14px 17px;
        border: 1px solid var(--border);
        border-radius: 12px;
        background: var(--surface);
        box-shadow: 0 10px 35px var(--soft-shadow);
        color: var(--text);
        font-size: 10px;
        line-height: 1.7;
    }
    .compact .article-stack {
        gap: 4px;
    }
    .compact .article-main {
        padding: 9px 12px;
    }
    .compact .article-visual {
        min-height: 110px;
        padding: 10px;
    }
    .compact .article-main h3 {
        margin: 6px 0 4px;
        line-height: 1.5;
    }
    .compact .article-footer {
        padding-top: 6px;
    }
    .compact .article-excerpt-collapsed {
        -webkit-line-clamp: 1;
        line-clamp: 1;
    }
    .excerpts-hidden .article-visual {
        min-height: 0;
    }
    .excerpts-hidden .article-main h3 {
        margin-bottom: 0;
    }
    .excerpts-hidden .article-footer {
        padding-top: 8px;
    }
    :global(:root[data-theme="dark"]) .hero {
        border-color: #47483f;
        background: linear-gradient(
            120deg,
            #333247 0%,
            #263b38 51%,
            #44352f 100%
        );
    }
    :global(:root[data-theme="dark"]) .hero h1,
    :global(:root[data-theme="dark"]) .profile-summary-name,
    :global(:root[data-theme="dark"]) .settings-summary-value {
        color: #f0eee8;
    }
    :global(:root[data-theme="dark"]) .hero-copy p,
    :global(:root[data-theme="dark"]) .eyebrow,
    :global(:root[data-theme="dark"]) .profile-summary-label,
    :global(:root[data-theme="dark"]) .settings-summary-label,
    :global(:root[data-theme="dark"]) .settings-summary-detail {
        color: #d0cec5;
    }
    :global(:root[data-theme="dark"]) .profile-summary,
    :global(:root[data-theme="dark"]) .settings-summary {
        border-color: rgba(255, 255, 255, 0.12);
        background: rgba(0, 0, 0, 0.15);
    }
    :global(:root[data-theme="dark"]) .profile-initial {
        border-color: rgba(255, 255, 255, 0.15);
        background: rgba(0, 0, 0, 0.16);
        color: #f0eee8;
    }
    :global(:root[data-theme="dark"]) .originals-hero {
        background: linear-gradient(
            120deg,
            #33273b 0%,
            #2b3045 45%,
            #263b36 100%
        );
    }
    :global(:root[data-theme="dark"]) .originals-hero .hero-copy p,
    :global(:root[data-theme="dark"]) .originals-hero .eyebrow {
        color: #e3d9ef;
    }
    :global(:root[data-theme="dark"]) .originals-hero-label {
        border-color: rgba(255, 255, 255, 0.12);
        background: rgba(255, 255, 255, 0.07);
        color: #e5d9f3;
    }
    :global(:root[data-theme="dark"]) .subscription-active-card {
        border-color: #41624e;
    }
    :global(:root[data-theme="dark"]) .subscription-symbol {
        border-color: #41624e;
        background: #26372c;
    }
    @media (min-width: 1450px) {
        .article-stack {
            display: grid;
            grid-template-columns: repeat(2, minmax(0, 1fr));
            align-items: start;
        }
        .article-capsule {
            grid-template-columns: 120px minmax(0, 1fr);
        }
        .article-visual {
            min-height: 185px;
        }
        .reader-page {
            padding-top: 32px;
        }
    }
    @media (max-width: 900px) {
        .workspace {
            padding-right: 24px;
            padding-left: 24px;
        }
        .topic-grid {
            grid-template-columns: repeat(3, minmax(0, 1fr));
        }
        .hero {
            padding: 27px;
        }
        .article-capsule {
            grid-template-columns: 112px minmax(0, 1fr);
        }
        .article-main {
            padding: 13px;
        }
    }
    @media (max-width: 650px) {
        .app-shell {
            grid-template-columns: 48px minmax(0, 1fr);
        }
        .sidebar {
            padding-top: 15px;
        }
        .brand {
            width: 42px;
            height: 34px;
            margin-left: 3px;
            border-radius: 11px;
        }
        .brand:hover,
        .brand:focus-visible {
            width: 207px;
        }
        .brand-mark {
            left: 8px;
            font-size: 21px;
        }
        .brand-equation {
            left: 38px;
            font-size: 10px;
        }
        .brand-equation em {
            font-size: 12px;
        }
        .region-nav {
            gap: 7px;
            margin-top: 22px;
            margin-left: 1px;
            padding: 9px 4px;
            border-radius: 22px;
        }
        .region-button {
            width: 32px;
            height: 34px;
        }
        .region-icon {
            font-size: 17px;
        }
        .workspace {
            padding: 15px 12px 45px;
        }
        .topbar {
            gap: 8px;
            margin-bottom: 22px;
        }
        .topbar-left {
            flex: 1 1 auto;
            gap: 8px;
        }
        .region-name {
            font-size: 11px;
            line-height: 1.6;
        }
        .context-topic {
            max-width: 90px;
            font-size: 9px;
            overflow-wrap: anywhere;
        }
        .topbar-right {
            flex: 0 0 auto;
            gap: 8px;
        }
        .wordmark-small {
            font-size: 9px;
        }
        .hero {
            min-height: 0;
            flex-direction: column;
            align-items: stretch;
            gap: 24px;
            padding: 22px 17px;
            border-radius: 17px;
        }
        .hero-copy {
            flex: initial;
        }
        .hero h1 {
            font-size: 30px;
            letter-spacing: -1.8px;
        }
        .hero-copy p {
            max-width: 370px;
            margin-top: 10px;
            font-size: 9px;
            line-height: 1.8;
        }
        .hero-code {
            flex: initial;
            width: 100%;
            padding: 12px;
        }
        .hero-code pre {
            min-height: 0;
            max-height: 220px;
            font-size: 9px;
        }
        .code-filename,
        .code-language {
            font-size: 8px;
        }
        .account-hero,
        .settings-hero {
            gap: 19px;
        }
        .profile-summary,
        .settings-summary {
            flex: initial;
            width: 100%;
            min-width: 0;
            padding: 13px;
        }
        .profile-initial {
            width: 39px;
            height: 39px;
            font-size: 18px;
        }
        .topic-section,
        .coverage-section,
        .more-section,
        .profile-section,
        .settings-section {
            margin-top: 28px;
        }
        .section-heading h2 {
            font-size: 12px;
        }
        .topic-grid {
            grid-template-columns: repeat(2, minmax(0, 1fr));
            gap: 8px;
        }
        .topic-card {
            min-height: 64px;
            padding: 12px;
            border-radius: 11px;
        }
        .topic-name {
            font-size: 10px;
        }
        .article-stack {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .article-capsule {
            grid-template-columns: minmax(0, 1fr);
            overflow: visible;
            border-radius: 14px;
        }
        .article-capsule.swiped {
            width: calc(100% - var(--capsule-rail-width));
        }
        .capsule-wrap {
            border-radius: 14px;
        }
        .article-visual {
            min-height: 64px;
            flex-direction: row;
            align-items: center;
            gap: 8px;
            padding: 10px 12px;
            border-radius: 13px 13px 0 0;
        }
        .article-topic,
        .article-region {
            font-size: 8px;
        }
        .article-main {
            padding: 13px;
        }
        .article-main h3 {
            margin-top: 8px;
            font-size: 12px;
        }
        .article-excerpt {
            font-size: 9px;
        }
        .article-footer {
            gap: 6px;
            padding-top: 12px;
        }
        .article-team {
            max-width: 60%;
            font-size: 8px;
        }
        .article-actions {
            gap: 9px;
        }
        .verification-tooltip {
            right: -3px;
            width: min(225px, calc(100vw - 95px));
        }
        .reader-page {
            padding-top: 8px;
        }
        .reader-kicker {
            margin-bottom: 16px;
            font-size: 9px;
        }
        .reader-title {
            font-size: clamp(25px, 7vw, 35px);
            letter-spacing: -1.4px;
        }
        .reader-deck {
            margin: 15px 0 21px;
            font-size: 11px;
            line-height: 1.9;
        }
        .reader-byline {
            align-items: flex-start;
            flex-direction: column;
            gap: 13px;
            padding: 14px 0;
        }
        .reader-facts {
            grid-template-columns: minmax(0, 1fr);
            gap: 8px;
        }
        .reader-fact {
            min-height: 0;
            padding: 12px;
        }
        .reader-content {
            font-size: 11px;
            line-height: 2;
        }
        .reader-content p {
            margin-bottom: 20px;
        }
        .reader-rule {
            margin: 21px 0;
        }
        .profile-form {
            grid-template-columns: minmax(0, 1fr);
            padding: 10px;
            gap: 9px;
        }
        .profile-form-footer {
            grid-column: auto;
        }
        .password-rules {
            grid-template-columns: minmax(0, 1fr);
        }
        .phone-field {
            grid-column: auto;
        }
        .password-form {
            grid-template-columns: minmax(0, 1fr);
        }
        .subscription-offer,
        .subscription-active-card {
            padding: 15px;
        }
        .billing-form {
            grid-template-columns: minmax(0, 1fr);
            gap: 9px;
        }
        .billing-heading,
        .card-number-field,
        .billing-row,
        .subscribe-button,
        .billing-disclaimer {
            grid-column: auto;
        }
        .billing-row {
            display: grid;
            grid-template-columns: repeat(2, minmax(0, 1fr));
        }
        .subscription-details {
            grid-template-columns: minmax(0, 1fr);
            gap: 14px;
        }
        .offer-benefits,
        .paywall-features {
            grid-template-columns: minmax(0, 1fr);
            gap: 10px;
        }
        .editor-select-grid {
            grid-template-columns: minmax(0, 1fr);
            gap: 9px;
        }
        .more-grid,
        .more-article-grid {
            grid-template-columns: minmax(0, 1fr);
            gap: 8px;
        }
        .more-card {
            min-height: 59px;
        }
        .more-article-card {
            min-height: 110px;
        }
        .paywall-section {
            padding: 30px 17px;
        }
        .setting-card {
            gap: 11px;
            padding: 13px;
        }
        .setting-copy h3 {
            font-size: 11px;
        }
        .setting-copy p {
            font-size: 9px;
        }
        .setting-value {
            font-size: 8px;
            line-height: 1.6;
        }
        .dialog-card {
            padding: 22px 16px;
        }
        .toast {
            right: 12px;
            bottom: 12px;
        }
    }
    @media (prefers-reduced-motion: reduce) {
        *,
        *::before,
        *::after {
            scroll-behavior: auto !important;
            animation-duration: 0.01ms !important;
            animation-iteration-count: 1 !important;
            transition-duration: 0.01ms !important;
        }
    }

    .sidebar {
        overflow-x: hidden;
        overflow-y: auto;
        overscroll-behavior: contain;
        scrollbar-width: thin;
        scrollbar-color: var(--border-strong) transparent;
    }
    .brand-wrap {
        position: relative;
        z-index: 40;
        flex: 0 0 auto;
    }
    .brand-wrap:hover,
    .brand-wrap:focus-within {
        position: fixed;
        top: 24px;
        left: 12px;
        z-index: 90;
    }
    .brand-wrap:hover .brand,
    .brand-wrap:focus-within .brand {
        margin-left: 0;
    }
    .brand-info {
        position: absolute;
        top: 51px;
        left: 0;
        z-index: 2;
        display: flex;
        width: 242px;
        flex-direction: column;
        gap: 2px;
        padding: 9px;
        border: 1px solid var(--border);
        border-radius: 13px;
        background: var(--surface);
        box-shadow: 0 12px 34px var(--soft-shadow);
        opacity: 0;
        visibility: hidden;
        transform: translateY(-5px);
        transition:
            opacity 0.16s ease,
            transform 0.16s ease,
            visibility 0.16s ease;
        pointer-events: none;
    }
    .brand-wrap:hover .brand-info,
    .brand-wrap:focus-within .brand-info {
        opacity: 1;
        visibility: visible;
        transform: translateY(0);
        pointer-events: auto;
    }
    .brand-info a {
        display: flex;
        justify-content: space-between;
        gap: 12px;
        padding: 8px 9px;
        border-radius: 7px;
        color: var(--text-secondary);
        text-decoration: none;
        font-size: 9px;
    }
    .brand-info a:hover,
    .brand-info a:focus-visible {
        background: var(--surface-soft);
        color: var(--text);
    }
    .brand-info a span {
        color: var(--muted);
        text-align: right;
        overflow-wrap: anywhere;
    }
    .region-nav {
        flex: 0 0 auto;
    }
    .profile-summary {
        position: relative;
    }
    .blacklist-message {
        display: flex;
        max-width: 300px;
        flex-direction: column;
        gap: 7px;
        padding: 11px;
        border: 1px solid #e8a6aa;
        border-radius: 10px;
        background: #fff0f0;
        color: #77323a;
        font-size: 8px;
        line-height: 1.75;
    }
    .blacklist-message strong {
        color: #8e2933;
        font-size: 9px;
    }
    .blacklist-message a {
        color: #9a2633;
        font-weight: 700;
    }
    .banned-badge {
        border-color: #d36e78;
        background: #fff0f0;
        color: #9a2633;
        cursor: pointer;
    }
    .role-note {
        color: var(--muted);
        font-size: 8px;
    }
    .error-text {
        color: var(--error) !important;
    }
    .success-text {
        color: #39805b;
    }
    .inline-warning {
        margin: 0;
        padding: 9px 11px;
        border: 1px solid #ead7a8;
        border-radius: 9px;
        background: #fff8e7;
        color: #8d681e;
        font-size: 9px;
        line-height: 1.7;
    }
    .error-bubble {
        margin: 0;
        padding: 10px 12px;
        border: 1px solid color-mix(in srgb, var(--error) 45%, transparent);
        border-radius: 11px;
        background: color-mix(in srgb, var(--error) 9%, var(--surface));
        color: var(--error);
        font-size: 9px;
        line-height: 1.7;
    }
    .download-error {
        position: fixed;
        right: 22px;
        bottom: 22px;
        z-index: 130;
        width: min(420px, calc(100vw - 44px));
        box-shadow: 0 10px 35px var(--soft-shadow);
    }
    .resource-editor {
        display: flex;
        min-width: 0;
        flex-direction: column;
        gap: 10px;
        padding: 17px;
        border: 1px solid var(--border);
        border-radius: 15px;
        background: var(--surface);
    }
    .resource-editor-heading {
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 13px;
    }
    .resource-editor-heading > div {
        display: flex;
        flex-direction: column;
        gap: 5px;
    }
    .resource-editor-heading strong {
        color: var(--text);
        font-size: 10px;
    }
    .resource-editor-heading span {
        color: var(--muted);
        font-size: 8px;
        line-height: 1.6;
    }
    .file-input-hidden {
        position: absolute !important;
        width: 1px !important;
        height: 1px !important;
        padding: 0 !important;
        overflow: hidden !important;
        clip: rect(0, 0, 0, 0) !important;
        white-space: nowrap !important;
        clip-path: inset(50%) !important;
    }
    .file-picker {
        display: inline-flex;
        width: fit-content;
        min-height: 37px;
        align-items: center;
        justify-content: center;
        gap: 8px;
        padding: 9px 13px;
        border: 1px solid var(--border-strong);
        border-radius: 9px;
        background: var(--surface-soft);
        color: var(--text);
        font-size: 9px;
        font-weight: 700;
        cursor: pointer;
        transition:
            border-color 0.16s ease,
            background 0.16s ease,
            transform 0.16s ease;
    }
    .file-picker.invalid,
    .file-picker.invalid:hover {
        border-color: var(--error);
        box-shadow: 0 0 0 1px color-mix(in srgb, var(--error) 28%, transparent);
        color: var(--error);
    }
    .file-picker:hover,
    .file-picker:focus-within {
        border-color: #8c86d1;
        background: var(--surface);
        transform: translateY(-1px);
    }
    .file-input-hidden:focus-visible + .file-picker {
        outline: 2px solid #7479dc;
        outline-offset: 3px;
    }
    .attachment-list {
        display: flex;
        flex-wrap: wrap;
        gap: 7px;
    }
    .attachment-chip {
        display: inline-flex;
        max-width: 100%;
        align-items: center;
        gap: 9px;
        padding: 6px 7px 6px 10px;
        border: 1px solid var(--border);
        border-radius: 9px;
        background: var(--surface-soft);
        color: var(--text-secondary);
        font-size: 8px;
        line-height: 1.6;
    }
    .attachment-chip > span {
        min-width: 0;
        overflow-wrap: anywhere;
    }
    .attachment-chip small {
        margin-left: 5px;
        color: var(--muted);
        font-size: 7px;
        white-space: nowrap;
    }
    .attachment-chip button {
        display: grid;
        width: 22px;
        height: 22px;
        flex: 0 0 22px;
        place-items: center;
        border: 1px solid var(--border);
        border-radius: 7px;
        background: var(--surface);
        color: var(--muted);
        font-size: 15px;
        cursor: pointer;
    }
    .attachment-chip button:hover {
        border-color: var(--error);
        color: var(--error);
    }
    .custom-select {
        position: relative;
        z-index: 3;
    }
    .select-trigger {
        display: flex;
        width: 100%;
        min-height: 41px;
        align-items: center;
        justify-content: space-between;
        gap: 10px;
        padding: 9px 11px;
        border: 1px solid var(--border);
        border-radius: 9px;
        background: var(--input-bg);
        color: var(--text);
        text-align: left;
        font-size: 10px;
        cursor: pointer;
    }
    .select-trigger:hover,
    .select-trigger[aria-expanded="true"] {
        border-color: var(--border-strong);
        box-shadow: 0 4px 14px var(--soft-shadow);
    }
    .select-chevron {
        flex: 0 0 auto;
        color: var(--muted);
        font-size: 14px;
    }
    .select-menu,
    .topic-select-menu {
        position: absolute;
        top: calc(100% + 7px);
        left: 0;
        z-index: 25;
        display: flex;
        width: 100%;
        max-height: 265px;
        flex-direction: column;
        gap: 3px;
        overflow-y: auto;
        padding: 7px;
        border: 1px solid var(--border);
        border-radius: 12px;
        background: var(--surface);
        box-shadow: 0 13px 34px var(--soft-shadow);
    }
    .region-option {
        display: flex;
        min-height: 34px;
        align-items: center;
        gap: 9px;
        padding: 7px 9px;
        border: 1px solid transparent;
        border-radius: 8px;
        background: transparent;
        color: var(--text-secondary);
        text-align: left;
        font-size: 9px;
        cursor: pointer;
    }
    .region-option:hover,
    .region-option.selected {
        border-color: var(--border);
        background: var(--surface-soft);
        color: var(--text);
    }
    .topic-select-trigger {
        padding: 6px 9px;
    }
    .selected-topic-pill,
    .topic-option {
        display: inline-flex;
        width: fit-content;
        max-width: 100%;
        align-items: center;
        padding: 6px 10px;
        border: 1px solid rgba(255, 255, 255, 0.65);
        border-radius: 999px;
        background: var(--topic-gradient);
        color: #fff;
        font-size: 8px;
        font-weight: 700;
        line-height: 1.3;
    }
    .topic-select-menu {
        align-items: flex-start;
    }
    .topic-option {
        min-height: 27px;
        cursor: pointer;
        box-shadow: inset 0 1px rgba(255, 255, 255, 0.18);
        transition:
            transform 0.14s ease,
            filter 0.14s ease,
            box-shadow 0.14s ease;
    }
    .topic-option:hover {
        transform: translateX(2px);
        filter: saturate(1.12);
    }
    .topic-option.selected {
        outline: 2px solid var(--text);
        outline-offset: 1px;
    }
    .editor-form {
        gap: 12px;
    }
    .editor-form > .profile-field {
        width: 100%;
    }
    .editor-select-grid {
        position: relative;
        z-index: 4;
    }
    .editor-select-grid > .profile-field {
        overflow: visible;
    }
    .restore-button {
        display: inline-flex;
        min-height: 33px;
        align-items: center;
        justify-content: center;
        padding: 7px 13px;
        border: 1px solid var(--border-strong);
        border-radius: 8px;
        background: var(--surface-soft);
        color: var(--text);
        font-size: 9px;
        font-weight: 700;
        cursor: pointer;
        transition:
            border-color 0.15s ease,
            background 0.15s ease,
            transform 0.15s ease;
    }
    .restore-button:hover {
        border-color: #78a98a;
        background: #edf7f0;
        color: #276648;
        transform: translateY(-1px);
    }
    .empty-deleted {
        display: flex;
        align-items: center;
        gap: 11px;
        padding: 19px;
        border: 1px dashed var(--border-strong);
        border-radius: 14px;
        color: var(--muted);
        font-size: 10px;
    }
    .empty-deleted p {
        margin: 0;
    }
    .capsule-wrap {
        --capsule-rail-width: 66px;
        isolation: isolate;
    }
    .capsule-lip-trigger {
        position: absolute;
        top: 50%;
        right: 0;
        z-index: 4;
        display: grid;
        width: 23px;
        height: 46px;
        place-items: center;
        padding: 0;
        border: 1px solid var(--border-strong);
        border-right: 0;
        border-radius: 12px 0 0 12px;
        background: var(--surface-soft);
        color: var(--text-secondary);
        font-size: 17px;
        font-weight: 700;
        opacity: 0;
        transform: translate(7px, -50%);
        transition:
            opacity 0.15s ease,
            transform 0.15s ease,
            background 0.15s ease;
        cursor: pointer;
    }
    .capsule-lip-trigger:hover,
    .capsule-lip-trigger:focus-visible {
        opacity: 1;
        transform: translate(0, -50%);
    }
    .capsule-wrap.actions-open .capsule-actions {
        opacity: 1;
        visibility: visible;
        pointer-events: auto;
    }
    .article-open-button {
        position: absolute;
        inset: 0;
        z-index: 2;
        width: 100%;
        height: 100%;
        margin: 0;
        padding: 0;
        border: 0;
        border-radius: inherit;
        background: transparent;
        cursor: pointer;
        touch-action: pan-y;
    }
    .article-open-button:focus-visible {
        outline-offset: -3px;
    }
    .dialog-card:focus {
        outline: none;
    }
    .article-capsule:hover .article-excerpt-collapsed,
    .article-capsule:focus-within .article-excerpt-collapsed {
        display: block;
        -webkit-line-clamp: unset;
        line-clamp: unset;
    }
    .capsule-wrap:has(.verification-wrap:hover),
    .capsule-wrap:has(.verification-wrap:focus-within) {
        z-index: 10;
    }
    .password-rules .rule-failed {
        color: var(--error);
    }
    button:disabled {
        opacity: 0.5;
        cursor: not-allowed;
    }
    .like-button:disabled:hover .heart-icon {
        transform: none;
        color: var(--muted);
    }
    .profile-field input:disabled {
        color: var(--muted);
        cursor: not-allowed;
    }
    .capsule-actions button {
        color: var(--text-secondary);
        transition:
            background 0.15s ease,
            color 0.15s ease,
            transform 0.15s ease;
    }
    .capsule-actions button:hover {
        color: var(--text);
        transform: scale(1.08);
    }
    .verification-tooltip {
        position: fixed;
        top: var(--tooltip-top, 10px);
        right: auto;
        bottom: auto;
        left: var(--tooltip-left, 10px);
        z-index: 150;
        max-height: calc(100vh - 20px);
        overflow-y: auto;
        pointer-events: none;
    }
    .verification-wrap:hover .verification-tooltip,
    .verification-wrap:focus-within .verification-tooltip {
        pointer-events: auto;
    }
    .verification-tooltip a {
        display: block;
        max-width: 100%;
        color: #5974bb;
        font-size: 8px;
        overflow-wrap: anywhere;
        pointer-events: auto;
    }
    .tooltip-downloads {
        display: flex;
        flex-direction: column;
        gap: 5px;
        margin-top: 3px;
        pointer-events: auto;
    }
    .tooltip-download {
        width: 100%;
        padding: 6px 7px;
        border: 1px solid var(--border);
        border-radius: 7px;
        background: var(--surface-soft);
        color: var(--text-secondary);
        text-align: left;
        font-size: 8px;
        overflow-wrap: anywhere;
        cursor: pointer;
        pointer-events: auto;
    }
    .tooltip-download:hover {
        border-color: var(--border-strong);
        color: var(--text);
    }
    .reader-resources {
        display: grid;
        gap: 15px;
        max-width: 720px;
        margin: 28px auto;
        padding: 17px;
        border: 1px solid var(--border);
        border-radius: 14px;
        background: var(--surface);
    }
    .reader-resources h2 {
        margin: 0 0 9px;
        color: var(--text);
        font-size: 10px;
    }
    .reader-resources ul {
        display: flex;
        flex-direction: column;
        gap: 7px;
        margin: 0;
        padding-left: 17px;
    }
    .reader-resources a {
        color: #5872b5;
        font-size: 9px;
        line-height: 1.7;
        overflow-wrap: anywhere;
    }
    .reader-attachments {
        display: flex;
        flex-wrap: wrap;
        gap: 7px;
    }
    .download-button {
        display: inline-flex;
        min-height: 32px;
        align-items: center;
        gap: 6px;
        padding: 7px 10px;
        border: 1px solid var(--border);
        border-radius: 8px;
        background: var(--surface-soft);
        color: var(--text-secondary);
        font-size: 8px;
        cursor: pointer;
    }
    .download-button:hover {
        border-color: var(--border-strong);
        color: var(--text);
    }
    .report-confirmation {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 12px 5px 4px;
        text-align: center;
    }
    .report-logo {
        display: grid;
        width: 50px;
        height: 50px;
        place-items: center;
        border: 1px solid var(--border);
        border-radius: 16px;
        background: var(--surface-soft);
        color: var(--text);
        font-size: 25px;
        font-weight: 800;
        letter-spacing: -4px;
    }
    .report-confirmation h2 {
        margin-top: 20px;
    }
    .report-confirmation p {
        max-width: 440px;
        margin: 12px 0 18px;
        color: var(--text-secondary);
        font-size: 10px;
        line-height: 1.9;
    }
    .toast {
        border-radius: 12px;
    }
    :global(:root[data-theme="dark"]) .blacklist-message {
        border-color: #774149;
        background: #3b2427;
        color: #f0c8cc;
    }
    :global(:root[data-theme="dark"]) .blacklist-message strong,
    :global(:root[data-theme="dark"]) .blacklist-message a {
        color: #ff9ca5;
    }
    :global(:root[data-theme="dark"]) .banned-badge {
        border-color: #774149;
        background: #3b2427;
        color: #ff9ca5;
    }
    :global(:root[data-theme="dark"]) .inline-warning {
        border-color: #68572d;
        background: #373020;
        color: #e8cc82;
    }
    :global(:root[data-theme="dark"]) .restore-button:hover {
        border-color: #41624e;
        background: #26372c;
        color: #b5e0c4;
    }
    :global(:root[data-theme="dark"]) .error-bubble {
        background: color-mix(in srgb, var(--error) 16%, var(--surface));
    }
    @media (max-width: 650px) {
        .app-shell {
            display: block;
            min-height: 100vh;
            padding-bottom: 78px;
        }
        .sidebar {
            position: fixed;
            inset: auto 0 0;
            z-index: 80;
            display: flex;
            width: 100%;
            height: 70px;
            flex-direction: row;
            align-items: center;
            gap: 9px;
            padding: 7px 10px;
            border-top: 1px solid var(--border);
            border-right: 0;
            background: color-mix(in srgb, var(--page) 95%, transparent);
            overflow-x: auto;
            overflow-y: hidden;
            overscroll-behavior-x: contain;
            scrollbar-width: thin;
        }
        .brand-wrap {
            position: relative;
            flex: 0 0 auto;
        }
        .brand-wrap:hover,
        .brand-wrap:focus-within {
            position: fixed;
            top: auto;
            bottom: 78px;
            left: 10px;
        }
        .brand {
            width: 42px;
            height: 40px;
            margin-left: 0;
            border-radius: 12px;
        }
        .brand:hover,
        .brand:focus-visible {
            width: 207px;
        }
        .brand-info {
            top: auto;
            bottom: 50px;
            left: 0;
            width: min(250px, calc(100vw - 22px));
        }
        .region-nav {
            width: max-content;
            flex-direction: row;
            gap: 6px;
            margin: 0;
            padding: 5px 7px;
            border-radius: 20px;
        }
        .region-button {
            width: 35px;
            height: 38px;
            flex: 0 0 35px;
        }
        .workspace {
            padding: 15px 12px 45px;
        }
        .capsule-wrap {
            --capsule-rail-width: 48px;
        }
        .capsule-lip-trigger {
            top: 50%;
            right: 0;
            width: 22px;
            height: 42px;
            opacity: 1;
            transform: translateY(-50%);
        }
        .compact .article-visual {
            min-height: 42px;
            padding: 7px 10px;
        }
        .compact .article-main {
            padding: 8px 11px;
        }
        .compact .article-main h3 {
            margin-top: 4px;
            margin-bottom: 3px;
        }
        .excerpts-hidden .article-visual {
            min-height: 0;
        }
        .compact.excerpts-hidden .article-visual {
            min-height: 0;
        }
        .editor-form {
            gap: 9px;
        }
        .resource-editor {
            padding: 12px;
        }
        .resource-editor-heading {
            align-items: flex-start;
        }
        .file-picker {
            min-height: 36px;
        }
        .report-confirmation {
            padding-top: 5px;
        }
        .download-error {
            right: 12px;
            bottom: 83px;
            width: calc(100vw - 24px);
        }
    }

    .editor-select-grid.roster-open {
        z-index: 30;
    }
    .writer-search-capsule {
        padding: 18px;
        border: 1px solid var(--border);
        border-radius: 17px;
        background: var(--surface);
    }
    .writer-search-fields {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 12px;
    }
    .writer-groups {
        display: flex;
        flex-direction: column;
        gap: 23px;
        margin-top: 25px;
    }
    .writer-letter-heading {
        display: flex;
        align-items: center;
        gap: 13px;
        margin-bottom: 12px;
    }
    .writer-letter-heading h3 {
        flex: 0 0 auto;
        margin: 0;
        color: var(--text);
        font-size: 12px;
        font-weight: 700;
    }
    .writer-letter-heading::after {
        flex: 1;
        height: 1px;
        background: var(--border);
        content: "";
    }
    .writer-grid {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
    }
    .topic-card.writer-card {
        min-height: 36px;
        max-width: 100%;
        flex: 0 1 auto;
        align-items: center;
        padding: 9px 14px;
        border-radius: 999px;
    }
    .writer-card .topic-name {
        font-size: 10px;
        line-height: 1.5;
    }
    .writer-membership {
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-wrap: wrap;
        gap: 16px;
        margin-top: 17px;
        padding: 18px;
        border: 1px solid var(--border);
        border-radius: 17px;
        background: var(--surface);
    }
    .writer-membership > div {
        flex: 1 1 240px;
    }
    .writer-membership strong {
        color: var(--text);
        font-size: 11px;
    }
    .writer-membership p {
        margin: 8px 0 0;
        color: var(--text-secondary);
        font-size: 10px;
        line-height: 1.8;
    }
    @media (max-width: 650px) {
        .writer-search-capsule {
            padding: 12px;
        }
        .writer-search-fields {
            grid-template-columns: minmax(0, 1fr);
            gap: 9px;
        }
        .writer-groups {
            gap: 20px;
            margin-top: 22px;
        }
        .writer-grid {
            gap: 7px;
        }
        .writer-membership {
            padding: 15px;
        }
    }
</style>
