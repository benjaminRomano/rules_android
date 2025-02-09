load(":defs.bzl", "android_jar")

android_jar(
    name = "android_jar",
    visibility = ["//visibility:public"],
)

# TODO(b/175833857): This is a stub, should remove.
alias(
    name = "merge_manifests",
    actual = ":fail",
    visibility = ["//visibility:public"],
)

alias(
    name = "debug_keystore",
    actual = "@bazel_tools//tools/android:debug_keystore",
    visibility = ["//visibility:public"],
)

genrule(
    name = "gen_fail",
    outs = ["fail.sh"],
    cmd = "echo 'exit 1' > $@",
    executable = 1,
    visibility = ["//visibility:public"],
)

sh_binary(
    name = "fail",
    srcs = [":fail.sh"],
)

java_binary(
    name = "bundletool",
    main_class = "com.android.tools.build.bundletool.BundleToolMain",
    visibility = ["//visibility:public"],
    runtime_deps = ["@rules_android_maven//:com_android_tools_build_bundletool"],
)

java_plugin(
    name = "compiler_annotation_processor",
    generates_api = True,
    processor_class = "android.databinding.annotationprocessor.ProcessDataBinding",
    visibility = ["//visibility:public"],
    deps = [
        "@bazel_tools//src/tools/android/java/com/google/devtools/build/android:all_android_tools",
    ],
)

genrule(
    name = "full_desugar_jdk_libs_config_json",
    outs = ["full_desugar_jdk_libs_config.json"],
    srcs = [
        "@rules_android_maven//:com_android_tools_desugar_jdk_libs_configuration_nio",
    ],
    cmd = "unzip -q -c " +
          "$(location @rules_android_maven//:com_android_tools_desugar_jdk_libs_configuration_nio) " +
          "META-INF/desugar/d8/desugar.json > $@",
)

genrule(
    name = "minify_desugar_jdk_libs_pgcfg",
    outs = ["minify_desugar_jdk_libs.pgcfg"],
    srcs = [
        "//src/tools/extract_desugar_pgcfg_flags",
        ":full_desugar_jdk_libs_config_json",
    ],
    cmd = "$(location //src/tools/extract_desugar_pgcfg_flags) --input_json $(location :full_desugar_jdk_libs_config_json) --output_file $@"
)

alias(
    name = "java8_legacy_dex",
    actual = ":gen_fail",
    visibility = ["//visibility:public"],
)

alias(
    name = "desugar_java8",
    actual = "@bazel_tools//tools/android:desugar_java8",
    visibility = ["//visibility:public"],
)

alias(
    name = "desugared_java8_legacy_apis",
    actual = ":gen_fail",  # TODO(#122): Fix library desugaring
    visibility = ["//visibility:public"],
)

java_binary(
    name = "d8",
    main_class = "com.android.tools.r8.D8",
    visibility = ["//visibility:public"],
    runtime_deps = ["@android_gmaven_r8//jar"],
)

java_binary(
    name = "r8",
    main_class = "com.android.tools.r8.R8",
    visibility = ["//visibility:public"],
    runtime_deps = ["@android_gmaven_r8//jar"],
)

java_binary(
    name = "resource_shrinker",
    main_class = "com.android.build.shrinker.ResourceShrinkerCli",
    visibility = ["//visibility:public"],
    runtime_deps = ["@rules_android_maven//:com_android_tools_build_gradle"],
)

alias(
    name = "dexbuilder",
    actual = "@bazel_tools//tools/android:dexbuilder",
    visibility = ["//visibility:public"],
)

alias(
    name = "dexmerger",
    actual = "@bazel_tools//tools/android:dexmerger",
    visibility = ["//visibility:public"],
)
