SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor.dieunkrauts.de/tap-wkld/tanzu-java-web-app-test-marco-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')

k8s_custom_deploy(
    'tanzu-java-web-app-test-marco',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --debug --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes " +
               OUTPUT_TO_NULL_COMMAND +
               " && kubectl get workload tanzu-java-web-app-test-marco --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('tanzu-java-web-app-test-marco', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'tanzu-java-web-app-test-marco'}])
